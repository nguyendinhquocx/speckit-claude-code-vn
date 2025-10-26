---
description: Thực thi workflow planning triển khai sử dụng plan template để generate các design artifacts.
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Outline

1. **Setup**: Chạy `.specify/scripts/powershell/setup-plan.ps1 -Json` từ repo root và parse JSON cho FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

2. **Load context**: Đọc FEATURE_SPEC và `.specify/memory/constitution.md`. Load IMPL_PLAN template (đã được copied).

3. **Execute plan workflow**: Follow cấu trúc trong IMPL_PLAN template để:
   - Điền Technical Context (đánh dấu unknowns là "CẦN LÀM RÕ")
   - Điền Constitution Check section từ constitution
   - Evaluate gates (ERROR nếu violations không justified)
   - Phase 0: Generate research.md (resolve tất cả CẦN LÀM RÕ)
   - Phase 1: Generate data-model.md, contracts/, quickstart.md
   - Phase 1: Update agent context bằng cách chạy agent script
   - Re-evaluate Constitution Check post-design

   **NGÔN NGỮ OUTPUT BẮT BUỘC**:
   - **TẤT CẢ tài liệu (research.md, data-model.md, quickstart.md) PHẢI TIẾNG VIỆT**
   - Giữ nguyên: tech stack names, library versions, code snippets, commands
   - Validation: "Các quyết định kỹ thuật có được giải thích bằng tiếng Việt không?"

4. **Dừng và report**: Command kết thúc sau Phase 2 planning. Report branch, IMPL_PLAN path, và các artifacts đã generate.

## Phases

### Phase 0: Outline & Research

1. **Extract unknowns từ Technical Context** ở trên:
   - Với mỗi CẦN LÀM RÕ → research task
   - Với mỗi dependency → best practices task
   - Với mỗi integration → patterns task

2. **Generate và dispatch research agents**:

   ```text
   Với mỗi unknown trong Technical Context:
     Task: "Research {unknown} cho {feature context}"
   Với mỗi technology choice:
     Task: "Tìm best practices cho {tech} trong {domain}"
   ```

3. **Consolidate findings** trong `research.md` sử dụng format:
   - Decision: [cái gì đã được chọn]
   - Rationale: [tại sao chọn]
   - Alternatives considered: [những gì khác đã được đánh giá]

**Output**: research.md với tất cả CẦN LÀM RÕ đã được resolve

### Phase 1: Design & Contracts

**Prerequisites:** `research.md` hoàn thành

1. **Extract entities từ feature spec** → `data-model.md`:
   - Entity name, fields, relationships
   - Validation rules từ requirements
   - State transitions nếu applicable

2. **Generate API contracts** từ functional requirements:
   - Với mỗi user action → endpoint
   - Dùng standard REST/GraphQL patterns
   - Output OpenAPI/GraphQL schema vào `/contracts/`

3. **Agent context update**:
   - Chạy `.specify/scripts/powershell/update-agent-context.ps1 -AgentType claude`
   - Các scripts này detect AI agent nào đang được sử dụng
   - Update agent-specific context file phù hợp
   - Chỉ add công nghệ mới từ plan hiện tại
   - Preserve manual additions giữa markers

**Output**: data-model.md, /contracts/*, quickstart.md, agent-specific file

## Key rules

- Dùng absolute paths
- ERROR khi gate failures hoặc unresolved clarifications
