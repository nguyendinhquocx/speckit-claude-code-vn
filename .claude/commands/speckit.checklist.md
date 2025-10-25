---
description: Generate custom checklist cho feature hiện tại dựa trên yêu cầu của user.
---

## Mục đích Checklist: "Unit Tests cho Tiếng Anh"

**KHÁI NIỆM CRITICAL**: Checklists là **UNIT TESTS CHO REQUIREMENTS WRITING** - chúng validate quality, clarity, và completeness của requirements trong một given domain.

**KHÔNG phải cho verification/testing**:

- ❌ KHÔNG "Verify button clicks correctly"
- ❌ KHÔNG "Test error handling works"
- ❌ KHÔNG "Confirm API returns 200"
- ❌ KHÔNG checking nếu code/implementation matches spec

**CHO requirements quality validation**:

- ✅ "Yêu cầu visual hierarchy có được định nghĩa cho tất cả card types không?" (completeness)
- ✅ "'Prominent display' có được quantified với specific sizing/positioning không?" (clarity)
- ✅ "Hover state requirements có consistent across tất cả interactive elements không?" (consistency)
- ✅ "Accessibility requirements có được định nghĩa cho keyboard navigation không?" (coverage)
- ✅ "Spec có define điều gì xảy ra khi logo image fails to load không?" (edge cases)

**Metaphor**: Nếu spec của mày là code được viết bằng tiếng Anh, checklist là unit test suite của nó. Mày đang test xem requirements có được viết tốt, đầy đủ, rõ ràng, và sẵn sàng cho implementation hay không - KHÔNG phải implementation có hoạt động không.

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Execution Steps

1. **Setup**: Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json` từ repo root và parse JSON cho FEATURE_DIR và AVAILABLE_DOCS list.
   - Tất cả file paths phải absolute.
   - Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

2. **Làm rõ intent (dynamic)**: Derive tối đa BA initial contextual clarifying questions (no pre-baked catalog). Chúng PHẢI:
   - Được generated từ phrasing của user + extracted signals từ spec/plan/tasks
   - Chỉ hỏi về thông tin materially changes checklist content
   - Được skipped individually nếu đã rõ ràng trong `$ARGUMENTS`
   - Ưu tiên precision over breadth

   [XEM FILE GỐC CHO GENERATION ALGORITHM CHI TIẾT - GIỮ NGUYÊN LOGIC]

3. **Hiểu user request**: Combine `$ARGUMENTS` + clarifying answers:
   - Derive checklist theme (ví dụ: security, review, deploy, ux)
   - Consolidate explicit must-have items mentioned by user
   - Map focus selections tới category scaffolding
   - Infer bất kỳ missing context từ spec/plan/tasks (KHÔNG hallucinate)

4. **Load feature context**: Đọc từ FEATURE_DIR:
   - spec.md: Feature requirements và scope
   - plan.md (nếu exists): Technical details, dependencies
   - tasks.md (nếu exists): Implementation tasks

5. **Generate checklist** - Tạo "Unit Tests cho Requirements":
   - Tạo `FEATURE_DIR/checklists/` directory nếu nó chưa exist
   - Generate unique checklist filename:
     - Dùng short, descriptive name dựa trên domain (ví dụ: `ux.md`, `api.md`, `security.md`)
     - Format: `[domain].md`
     - Nếu file exists, append vào existing file
   - Đánh số items tuần tự starting từ CHK001
   - Mỗi `/speckit.checklist` run tạo NEW file (không bao giờ overwrites existing checklists)

   **NGUYÊN TẮC CỐT LÕI - Test Requirements, Không phải Implementation**:

   Mỗi checklist item PHẢI evaluate REQUIREMENTS THEMSELVES cho:
   - **Completeness**: Tất cả necessary requirements có present không?
   - **Clarity**: Requirements có rõ ràng và specific không?
   - **Consistency**: Requirements có align với nhau không?
   - **Measurability**: Requirements có thể objectively verified không?
   - **Coverage**: Tất cả scenarios/edge cases có được addressed không?

   **CẤU TRÚC Category** - Group items theo requirement quality dimensions:
   - **Requirement Completeness** (Tất cả necessary requirements có được documented không?)
   - **Requirement Clarity** (Requirements có specific và rõ ràng không?)
   - **Requirement Consistency** (Requirements có align mà không conflicts không?)
   - **Acceptance Criteria Quality** (Success criteria có measurable không?)
   - **Scenario Coverage** (Tất cả flows/cases có được addressed không?)
   - **Edge Case Coverage** (Boundary conditions có được defined không?)
   - **Non-Functional Requirements** (Performance, Security, Accessibility, etc. - có được specified không?)
   - **Dependencies & Assumptions** (Có được documented và validated không?)
   - **Ambiguities & Conflicts** (Cái gì cần clarification?)

   **LÀM THẾ NÀO ĐỂ VIẾT CHECKLIST ITEMS - "Unit Tests cho Tiếng Anh"**:

   ❌ **SAI** (Testing implementation):
   - "Verify landing page displays 3 episode cards"
   - "Test hover states work on desktop"
   - "Confirm logo click navigates home"

   ✅ **ĐÚNG** (Testing requirements quality):
   - "Số lượng chính xác và layout của featured episodes có được specified không?" [Completeness]
   - "'Prominent display' có được quantified với specific sizing/positioning không?" [Clarity]
   - "Hover state requirements có consistent across tất cả interactive elements không?" [Consistency]
   - "Keyboard navigation requirements có được defined cho tất cả interactive UI không?" [Coverage]
   - "Fallback behavior có được specified khi logo image fails to load không?" [Edge Cases]
   - "Loading states có được defined cho asynchronous episode data không?" [Completeness]
   - "Spec có define visual hierarchy cho competing UI elements không?" [Clarity]

6. **Structure Reference**: Generate checklist following canonical template trong `.specify/templates/checklist-template.md`.

   **NGÔN NGỮ OUTPUT BẮT BUỘC**:
   - **MỌI checklist item PHẢI TIẾNG VIỆT**: "Yêu cầu X có được định nghĩa không?" KHÔNG PHẢI "Are X requirements defined?"
   - Giữ nguyên: CHK IDs, spec references ([Spec §FR-1]), quality markers ([Completeness], [Clarity])
   - Validation: "Checklist này có người Việt không biết tiếng Anh đọc được không?"

7. **Report**: Output full path tới created checklist, item count, và remind user rằng mỗi run tạo file mới.

## Ví dụ Checklist Types & Sample Items

**UX Requirements Quality:** `ux.md`

- "Visual hierarchy requirements có được defined với measurable criteria không? [Clarity, Spec §FR-1]"
- "Số lượng và positioning của UI elements có được explicitly specified không? [Completeness, Spec §FR-1]"
- "Interaction state requirements (hover, focus, active) có được consistently defined không? [Consistency]"
- "Accessibility requirements có được specified cho tất cả interactive elements không? [Coverage, Gap]"
- "Fallback behavior có được defined khi images fail to load không? [Edge Case, Gap]"
- "'Prominent display' có thể objectively measured không? [Measurability, Spec §FR-4]"

**API Requirements Quality:** `api.md`

- "Error response formats có được specified cho tất cả failure scenarios không? [Completeness]"
- "Rate limiting requirements có được quantified với specific thresholds không? [Clarity]"
- "Authentication requirements có consistent across tất cả endpoints không? [Consistency]"
- "Retry/timeout requirements có được defined cho external dependencies không? [Coverage, Gap]"
- "Versioning strategy có được documented trong requirements không? [Gap]"
