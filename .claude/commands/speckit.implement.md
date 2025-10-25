---
description: Thực thi implementation plan bằng cách process và execute tất cả tasks định nghĩa trong tasks.md
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Outline

1. Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` từ repo root và parse FEATURE_DIR và AVAILABLE_DOCS list. Tất cả paths phải absolute. Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

2. **Kiểm tra checklists status** (nếu FEATURE_DIR/checklists/ exists):
   - Scan tất cả checklist files trong thư mục checklists/
   - Với mỗi checklist, count:
     - Total items: Tất cả lines matching `- [ ]` hoặc `- [X]` hoặc `- [x]`
     - Completed items: Lines matching `- [X]` hoặc `- [x]`
     - Incomplete items: Lines matching `- [ ]`
   - Tạo status table:

     ```text
     | Checklist | Total | Hoàn thành | Chưa xong | Trạng thái |
     |-----------|-------|------------|-----------|------------|
     | ux.md     | 12    | 12         | 0         | ✓ PASS     |
     | test.md   | 8     | 5          | 3         | ✗ FAIL     |
     | security.md | 6   | 6          | 0         | ✓ PASS     |
     ```

   - Tính overall status:
     - **PASS**: Tất cả checklists có 0 incomplete items
     - **FAIL**: Một hoặc nhiều checklists có incomplete items

   - **Nếu bất kỳ checklist nào incomplete**:
     - Hiển thị table với incomplete item counts
     - **DỪNG** và hỏi: "Một số checklists chưa hoàn thành. Mày có muốn tiếp tục implementation không? (yes/no)"
     - Đợi user response trước khi continuing
     - Nếu user nói "no" hoặc "wait" hoặc "stop", halt execution
     - Nếu user nói "yes" hoặc "proceed" hoặc "continue", tiến tới bước 3

   - **Nếu tất cả checklists complete**:
     - Hiển thị table showing tất cả checklists passed
     - Tự động tiến tới bước 3

3. Load và analyze implementation context:
   - **BẮT BUỘC**: Đọc tasks.md cho complete task list và execution plan
   - **BẮT BUỘC**: Đọc plan.md cho tech stack, architecture, và file structure
   - **NẾU TỒN TẠI**: Đọc data-model.md cho entities và relationships
   - **NẾU TỒN TẠI**: Đọc contracts/ cho API specifications và test requirements
   - **NẾU TỒN TẠI**: Đọc research.md cho technical decisions và constraints
   - **NẾU TỒN TẠI**: Đọc quickstart.md cho integration scenarios

4. **Project Setup Verification**:
   - **BẮT BUỘC**: Tạo/verify ignore files dựa trên actual project setup:

   **Detection & Creation Logic**:
   - Check nếu command sau succeeds để xác định repo là git repo (tạo/verify .gitignore nếu đúng):

     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```

   - Check nếu Dockerfile* exists hoặc Docker trong plan.md → tạo/verify .dockerignore
   - Check nếu .eslintrc* hoặc eslint.config.* exists → tạo/verify .eslintignore
   - Check nếu .prettierrc* exists → tạo/verify .prettierignore
   - Check nếu .npmrc hoặc package.json exists → tạo/verify .npmignore (nếu publishing)
   - Check nếu terraform files (*.tf) exist → tạo/verify .terraformignore
   - Check nếu .helmignore needed (helm charts present) → tạo/verify .helmignore

   **Nếu ignore file đã tồn tại**: Verify nó chứa essential patterns, append missing critical patterns only
   **Nếu ignore file thiếu**: Tạo với full pattern set cho detected technology

   **Common Patterns theo Technology** (từ plan.md tech stack):
   - **Node.js/JavaScript/TypeScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`
   - **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `*.egg-info/`
   - **Java**: `target/`, `*.class`, `*.jar`, `.gradle/`, `build/`
   - **C#/.NET**: `bin/`, `obj/`, `*.user`, `*.suo`, `packages/`
   - **Go**: `*.exe`, `*.test`, `vendor/`, `*.out`
   - **Ruby**: `.bundle/`, `log/`, `tmp/`, `*.gem`, `vendor/bundle/`
   - **PHP**: `vendor/`, `*.log`, `*.cache`, `*.env`
   - **Rust**: `target/`, `debug/`, `release/`, `*.rs.bk`, `*.rlib`, `*.prof*`, `.idea/`, `*.log`, `.env*`
   - **Kotlin**: `build/`, `out/`, `.gradle/`, `.idea/`, `*.class`, `*.jar`, `*.iml`, `*.log`, `.env*`
   - **C++**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.so`, `*.a`, `*.exe`, `*.dll`, `.idea/`, `*.log`, `.env*`
   - **C**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.a`, `*.so`, `*.exe`, `Makefile`, `config.log`, `.idea/`, `*.log`, `.env*`
   - **Swift**: `.build/`, `DerivedData/`, `*.swiftpm/`, `Packages/`
   - **R**: `.Rproj.user/`, `.Rhistory`, `.RData`, `.Ruserdata`, `*.Rproj`, `packrat/`, `renv/`
   - **Universal**: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.swp`, `.vscode/`, `.idea/`

   **Tool-Specific Patterns**:
   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
   - **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars`, `.terraform.lock.hcl`
   - **Kubernetes/k8s**: `*.secret.yaml`, `secrets/`, `.kube/`, `kubeconfig*`, `*.key`, `*.crt`

5. Parse cấu trúc tasks.md và extract:
   - **Task phases**: Setup, Tests, Core, Integration, Polish
   - **Task dependencies**: Sequential vs parallel execution rules
   - **Task details**: ID, description, file paths, parallel markers [P]
   - **Execution flow**: Order và dependency requirements

6. Execute implementation following task plan:
   - **Phase-by-phase execution**: Complete mỗi phase trước khi move sang tiếp theo
   - **Respect dependencies**: Chạy sequential tasks theo thứ tự, parallel tasks [P] có thể chạy together
   - **Follow TDD approach**: Execute test tasks trước corresponding implementation tasks của chúng
   - **File-based coordination**: Tasks affecting cùng files phải chạy sequentially
   - **Validation checkpoints**: Verify mỗi phase completion trước khi proceeding

7. Implementation execution rules:
   - **Setup first**: Khởi tạo project structure, dependencies, configuration
   - **Tests before code**: Nếu mày cần viết tests cho contracts, entities, và integration scenarios
   - **Core development**: Implement models, services, CLI commands, endpoints
   - **Integration work**: Database connections, middleware, logging, external services
   - **Polish and validation**: Unit tests, performance optimization, documentation

8. Progress tracking và error handling:
   - Report progress sau mỗi completed task
   - Halt execution nếu bất kỳ non-parallel task nào fails
   - Với parallel tasks [P], continue với successful tasks, report failed ones
   - Provide clear error messages với context cho debugging
   - Suggest next steps nếu implementation không thể proceed
   - **QUAN TRỌNG** Với completed tasks, đảm bảo đánh dấu task off là [X] trong tasks file.

9. Completion validation:
   - Verify tất cả required tasks đã completed
   - Check implemented features match original specification
   - Validate tests pass và coverage meets requirements
   - Confirm implementation follows technical plan
   - Report final status với summary của completed work

Lưu ý: Command này giả định complete task breakdown tồn tại trong tasks.md. Nếu tasks incomplete hoặc missing, suggest chạy `/speckit.tasks` trước để regenerate task list.
