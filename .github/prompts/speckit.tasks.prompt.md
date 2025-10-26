---
description: Generate tasks.md actionable, sắp xếp theo dependency cho tính năng dựa trên các design artifacts available.
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Outline

1. **Setup**: Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json` từ repo root và parse FEATURE_DIR và AVAILABLE_DOCS list. Tất cả paths phải absolute. Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

2. **Load design documents**: Đọc từ FEATURE_DIR:
   - **Required**: plan.md (tech stack, libraries, structure), spec.md (user stories với priorities)
   - **Optional**: data-model.md (entities), contracts/ (API endpoints), research.md (decisions), quickstart.md (test scenarios)
   - Note: Không phải tất cả projects đều có all documents. Generate tasks dựa trên những gì available.

3. **Execute task generation workflow**:
   - Load plan.md và extract tech stack, libraries, project structure
   - Load spec.md và extract user stories với priorities của chúng (P1, P2, P3, etc.)
   - Nếu data-model.md exists: Extract entities và map tới user stories
   - Nếu contracts/ exists: Map endpoints tới user stories
   - Nếu research.md exists: Extract decisions cho setup tasks
   - Generate tasks organized theo user story (xem Task Generation Rules bên dưới)
   - Generate dependency graph showing user story completion order
   - Tạo parallel execution examples per user story
   - Validate task completeness (mỗi user story có tất cả needed tasks, independently testable)

4. **Generate tasks.md**: Dùng `.specify/templates/tasks-template.md` làm structure, điền với:
   - Feature name đúng từ plan.md
   - Phase 1: Setup tasks (project initialization)
   - Phase 2: Foundational tasks (blocking prerequisites cho tất cả user stories)
   - Phase 3+: Một phase per user story (theo priority order từ spec.md)
   - Mỗi phase bao gồm: story goal, independent test criteria, tests (nếu requested), implementation tasks
   - Final Phase: Polish & cross-cutting concerns
   - Tất cả tasks phải follow strict checklist format (xem Task Generation Rules bên dưới)
   - File paths rõ ràng cho mỗi task
   - Dependencies section showing story completion order
   - Parallel execution examples per story
   - Implementation strategy section (MVP first, incremental delivery)

   **NGÔN NGỮ OUTPUT BẮT BUỘC**:
   - **MỌI task description PHẢI TIẾNG VIỆT**: "Tạo User model" KHÔNG PHẢI "Create User model"
   - Giữ nguyên: Task IDs (T001), markers ([P], [US1]), file paths
   - Validation: Scan tasks.md sau khi generate, nếu thấy "Create/Implement/Add" → SAI, phải "Tạo/Implement/Thêm"

5. **Report**: Output path tới generated tasks.md và summary:
   - Total task count
   - Task count per user story
   - Parallel opportunities identified
   - Independent test criteria cho mỗi story
   - Suggested MVP scope (typically chỉ User Story 1)
   - Format validation: Confirm TẤT CẢ tasks follow checklist format (checkbox, ID, labels, file paths)

Context cho task generation: $ARGUMENTS

File tasks.md phải immediately executable - mỗi task phải đủ specific để một LLM có thể complete nó mà không cần additional context.

## Task Generation Rules

**CRITICAL**: Tasks PHẢI được organized theo user story để enable independent implementation và testing.

**Tests là TÙY CHỌN**: Chỉ generate test tasks nếu explicitly requested trong feature specification hoặc nếu user requests TDD approach.

### Checklist Format (BẮT BUỘC)

Mọi task PHẢI strictly follow format này:

```text
- [ ] [TaskID] [P?] [Story?] Mô tả với file path
```

**Format Components**:

1. **Checkbox**: LUÔN LUÔN bắt đầu với `- [ ]` (markdown checkbox)
2. **Task ID**: Số tuần tự (T001, T002, T003...) theo execution order
3. **[P] marker**: Chỉ bao gồm NẾU task parallelizable (different files, không phụ thuộc vào incomplete tasks)
4. **[Story] label**: BẮT BUỘC CHỈ cho user story phase tasks
   - Format: [US1], [US2], [US3], etc. (maps tới user stories từ spec.md)
   - Setup phase: KHÔNG có story label
   - Foundational phase: KHÔNG có story label
   - User Story phases: PHẢI có story label
   - Polish phase: KHÔNG có story label
5. **Description**: Action rõ ràng với exact file path

**Ví dụ**:

- ✅ ĐÚNG: `- [ ] T001 Tạo cấu trúc project theo implementation plan`
- ✅ ĐÚNG: `- [ ] T005 [P] Implement authentication middleware trong src/middleware/auth.py`
- ✅ ĐÚNG: `- [ ] T012 [P] [US1] Tạo User model trong src/models/user.py`
- ✅ ĐÚNG: `- [ ] T014 [US1] Implement UserService trong src/services/user_service.py`
- ❌ SAI: `- [ ] Tạo User model` (thiếu ID và Story label)
- ❌ SAI: `T001 [US1] Tạo model` (thiếu checkbox)
- ❌ SAI: `- [ ] [US1] Tạo User model` (thiếu Task ID)
- ❌ SAI: `- [ ] T001 [US1] Tạo model` (thiếu file path)

### Task Organization

1. **Từ User Stories (spec.md)** - ORGANIZATION CHÍNH:
   - Mỗi user story (P1, P2, P3...) có phase riêng của nó
   - Map tất cả related components tới story của chúng:
     - Models needed cho story đó
     - Services needed cho story đó
     - Endpoints/UI needed cho story đó
     - Nếu tests requested: Tests specific cho story đó
   - Đánh dấu story dependencies (hầu hết stories nên independent)

2. **Từ Contracts**:
   - Map mỗi contract/endpoint → tới user story nó serves
   - Nếu tests requested: Mỗi contract → contract test task [P] trước implementation trong phase của story đó

3. **Từ Data Model**:
   - Map mỗi entity tới user story(ies) cần nó
   - Nếu entity serves nhiều stories: Đặt trong earliest story hoặc Setup phase
   - Relationships → service layer tasks trong appropriate story phase

4. **Từ Setup/Infrastructure**:
   - Shared infrastructure → Setup phase (Phase 1)
   - Foundational/blocking tasks → Foundational phase (Phase 2)
   - Story-specific setup → trong phase của story đó

### Phase Structure

- **Phase 1**: Setup (project initialization)
- **Phase 2**: Foundational (blocking prerequisites - PHẢI complete trước user stories)
- **Phase 3+**: User Stories theo priority order (P1, P2, P3...)
  - Trong mỗi story: Tests (nếu requested) → Models → Services → Endpoints → Integration
  - Mỗi phase nên là complete, independently testable increment
- **Final Phase**: Polish & Cross-Cutting Concerns
