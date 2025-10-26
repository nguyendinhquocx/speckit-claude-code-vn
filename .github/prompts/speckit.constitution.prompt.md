---
description: Tạo hoặc cập nhật project constitution từ interactive hoặc provided principle inputs, đảm bảo tất cả dependent templates stay in sync
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Outline

Mày đang update project constitution tại `.specify/memory/constitution.md`. File này là TEMPLATE chứa placeholder tokens trong square brackets (ví dụ: `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`). Công việc của mày là (a) collect/derive concrete values, (b) fill template precisely, và (c) propagate bất kỳ amendments nào across dependent artifacts.

Follow execution flow này:

1. Load existing constitution template tại `.specify/memory/constitution.md`.
   - Identify mọi placeholder token của form `[ALL_CAPS_IDENTIFIER]`.
   **QUAN TRỌNG**: User có thể require ít hoặc nhiều principles hơn những cái được dùng trong template. Nếu một number được specified, respect đó - follow general template. Mày sẽ update doc accordingly.

2. Collect/derive values cho placeholders:
   - Nếu user input (conversation) supplies một value, dùng nó.
   - Otherwise infer từ existing repo context (README, docs, prior constitution versions nếu embedded).
   - Cho governance dates: `RATIFICATION_DATE` là original adoption date (nếu unknown hỏi hoặc đánh dấu TODO), `LAST_AMENDED_DATE` là today nếu changes được made, otherwise giữ previous.
   - `CONSTITUTION_VERSION` phải increment theo semantic versioning rules:
     - MAJOR: Backward incompatible governance/principle removals hoặc redefinitions.
     - MINOR: New principle/section added hoặc materially expanded guidance.
     - PATCH: Clarifications, wording, typo fixes, non-semantic refinements.
   - Nếu version bump type ambiguous, propose reasoning trước khi finalizing.

3. Draft updated constitution content:
   - Thay thế mọi placeholder bằng concrete text (không có bracketed tokens left trừ intentionally retained template slots mà project đã chosen không define yet—explicitly justify bất kỳ cái nào left).
   - Preserve heading hierarchy và comments có thể removed once replaced trừ khi chúng vẫn add clarifying guidance.
   - Đảm bảo mỗi Principle section: succinct name line, paragraph (hoặc bullet list) capturing non‑negotiable rules, explicit rationale nếu không obvious.
   - Đảm bảo Governance section lists amendment procedure, versioning policy, và compliance review expectations.

4. Consistency propagation checklist (convert prior checklist thành active validations):
   - Đọc `.specify/templates/plan-template.md` và đảm bảo bất kỳ "Constitution Check" hoặc rules nào align với updated principles.
   - Đọc `.specify/templates/spec-template.md` cho scope/requirements alignment—update nếu constitution adds/removes mandatory sections hoặc constraints.
   - Đọc `.specify/templates/tasks-template.md` và đảm bảo task categorization reflects new hoặc removed principle-driven task types.
   - Đọc mỗi command file trong `.claude/commands/*.md` để verify không có outdated references remain.
   - Đọc bất kỳ runtime guidance docs nào (ví dụ: `README.md`, `docs/quickstart.md`). Update references tới principles changed.

5. Tạo Sync Impact Report (prepend như HTML comment ở top của constitution file sau update):
   - Version change: old → new
   - List của modified principles (old title → new title nếu renamed)
   - Added sections
   - Removed sections
   - Templates requiring updates (✅ updated / ⚠ pending) với file paths
   - Follow-up TODOs nếu bất kỳ placeholders nào intentionally deferred.

6. Validation trước final output:
   - Không còn unexplained bracket tokens.
   - Version line matches report.
   - Dates ISO format YYYY-MM-DD.
   - Principles là declarative, testable, và free of vague language.

7. Write completed constitution trở lại `.specify/memory/constitution.md` (overwrite).

8. Output final summary cho user với:
   - New version và bump rationale.
   - Bất kỳ files nào flagged cho manual follow-up.
   - Suggested commit message (ví dụ: `docs: amend constitution to vX.Y.Z (principle additions + governance update)`).

Formatting & Style Requirements:

- Dùng Markdown headings exactly như trong template (không demote/promote levels).
- Wrap long rationale lines để giữ readability (<100 chars ideally) nhưng không hard enforce với awkward breaks.
- Giữ single blank line giữa sections.
- Tránh trailing whitespace.

Nếu user supplies partial updates (ví dụ: chỉ một principle revision), vẫn perform validation và version decision steps.

Nếu critical info missing (ví dụ: ratification date truly unknown), insert `TODO(<FIELD_NAME>): explanation` và include trong Sync Impact Report dưới deferred items.

Không tạo template mới; luôn operate trên existing `.specify/memory/constitution.md` file.
