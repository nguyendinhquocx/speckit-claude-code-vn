---
description: Tạo checklist tùy chỉnh cho tính năng hiện tại dựa trên yêu cầu người dùng.
---

## Mục đích Checklist: "Unit Tests cho Tiếng Việt"

**KHÁI NIỆM QUAN TRỌNG**: Checklists là **UNIT TESTS CHO VIỆC VIẾT REQUIREMENTS** - chúng validate chất lượng, tính rõ ràng và đầy đủ của requirements trong một domain nhất định.

**KHÔNG dành cho verification/testing**:

- ❌ KHÔNG "Verify the button clicks correctly"
- ❌ KHÔNG "Test error handling works"
- ❌ KHÔNG "Confirm the API returns 200"
- ❌ KHÔNG kiểm tra xem code/implementation có match với spec không

**DÀNH CHO validation chất lượng requirements**:

- ✅ "Are visual hierarchy requirements defined for all card types?" (completeness)
- ✅ "Is 'prominent display' quantified with specific sizing/positioning?" (clarity)
- ✅ "Are hover state requirements consistent across all interactive elements?" (consistency)
- ✅ "Are accessibility requirements defined for keyboard navigation?" (coverage)
- ✅ "Does the spec define what happens when logo image fails to load?" (edge cases)

**Ẩn dụ**: Nếu spec của mày là code được viết bằng tiếng Việt, thì checklist chính là unit test suite của nó. Mày đang test xem requirements có được viết tốt, đầy đủ, không nhập nhằng và sẵn sàng cho implementation hay không - KHÔNG phải test xem implementation có hoạt động không.

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Các bước Thực thi

1. **Setup**: Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json` từ repo root và parse JSON cho FEATURE_DIR và AVAILABLE_DOCS list.
   - Tất cả file paths phải absolute.
   - Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

2. **Làm rõ intent (dynamic)**: Derive tối đa BA câu hỏi làm rõ ngữ cảnh ban đầu (không có catalog có sẵn). Chúng PHẢI:
   - Được generate từ cách diễn đạt của user + extract signals từ spec/plan/tasks
   - Chỉ hỏi về thông tin thay đổi nội dung checklist một cách quan trọng
   - Được skip riêng lẻ nếu đã rõ ràng trong `$ARGUMENTS`
   - Ưu tiên precision hơn breadth

   Algorithm generation:
   1. Extract signals: feature domain keywords (ví dụ: auth, latency, UX, API), risk indicators ("critical", "must", "compliance"), stakeholder hints ("QA", "review", "security team"), và explicit deliverables ("a11y", "rollback", "contracts").
   2. Cluster signals thành candidate focus areas (max 4) ranked theo relevance.
   3. Identify probable audience & timing (author, reviewer, QA, release) nếu không explicit.
   4. Detect missing dimensions: scope breadth, depth/rigor, risk emphasis, exclusion boundaries, measurable acceptance criteria.
   5. Formulate questions được chọn từ những archetype này:
      - Scope refinement (ví dụ: "Should this include integration touchpoints with X and Y or stay limited to local module correctness?")
      - Risk prioritization (ví dụ: "Which of these potential risk areas should receive mandatory gating checks?")
      - Depth calibration (ví dụ: "Is this a lightweight pre-commit sanity list or a formal release gate?")
      - Audience framing (ví dụ: "Will this be used by the author only or peers during PR review?")
      - Boundary exclusion (ví dụ: "Should we explicitly exclude performance tuning items this round?")
      - Scenario class gap (ví dụ: "No recovery flows detected—are rollback / partial failure paths in scope?")

   Question formatting rules:
   - Nếu present options, generate một compact table với columns: Option | Candidate | Why It Matters
   - Limit tối đa A–E options; omit table nếu free-form answer rõ ràng hơn
   - Không bao giờ hỏi user restate những gì họ đã nói
   - Tránh speculative categories (không hallucination). Nếu uncertain, hỏi explicitly: "Confirm whether X belongs in scope."

   Defaults khi không thể interaction:
   - Depth: Standard
   - Audience: Reviewer (PR) nếu code-related; Author otherwise
   - Focus: Top 2 relevance clusters

   Output câu hỏi (label Q1/Q2/Q3). Sau answers: nếu ≥2 scenario classes (Alternate / Exception / Recovery / Non-Functional domain) vẫn unclear, mày CÓ THỂ hỏi tối đa HAI follow‑ups targeted nữa (Q4/Q5) với một justification một dòng cho mỗi cái (ví dụ: "Unresolved recovery path risk"). Không exceed 5 câu hỏi total. Skip escalation nếu user explicitly decline thêm.

3. **Hiểu user request**: Combine `$ARGUMENTS` + clarifying answers:
   - Derive checklist theme (ví dụ: security, review, deploy, ux)
   - Consolidate explicit must-have items được đề cập bởi user
   - Map focus selections tới category scaffolding
   - Infer bất kỳ missing context nào từ spec/plan/tasks (KHÔNG hallucinate)

4. **Load feature context**: Đọc từ FEATURE_DIR:
   - spec.md: Feature requirements và scope
   - plan.md (nếu exists): Technical details, dependencies
   - tasks.md (nếu exists): Implementation tasks

   **Context Loading Strategy**:
   - Chỉ load những phần cần thiết relevant tới active focus areas (tránh full-file dumping)
   - Ưu tiên summarize các sections dài thành concise scenario/requirement bullets
   - Dùng progressive disclosure: thêm follow-on retrieval chỉ khi detect gaps
   - Nếu source docs lớn, generate interim summary items thay vì embed raw text

5. **Generate checklist** - Tạo "Unit Tests cho Requirements":
   - Tạo `FEATURE_DIR/checklists/` directory nếu chưa tồn tại
   - Generate unique checklist filename:
     - Dùng short, descriptive name dựa trên domain (ví dụ: `ux.md`, `api.md`, `security.md`)
     - Format: `[domain].md`
     - Nếu file exists, append vào existing file
   - Number items tuần tự bắt đầu từ CHK001
   - Mỗi `/speckit.checklist` run tạo một file MỚI (không bao giờ overwrite existing checklists)

   **NGUYÊN TẮC CỐT LÕI - Test Requirements, Không phải Implementation**:
   Mọi checklist item PHẢI evaluate chính BẢN THÂN REQUIREMENTS cho:
   - **Completeness**: Tất cả requirements cần thiết có present không?
   - **Clarity**: Requirements có unambiguous và specific không?
   - **Consistency**: Requirements có align với nhau không?
   - **Measurability**: Requirements có thể objectively verify được không?
   - **Coverage**: Tất cả scenarios/edge cases có được address không?

   **Category Structure** - Group items theo requirement quality dimensions:
   - **Requirement Completeness** (Tất cả requirements cần thiết có được document không?)
   - **Requirement Clarity** (Requirements có specific và unambiguous không?)
   - **Requirement Consistency** (Requirements có align without conflicts không?)
   - **Acceptance Criteria Quality** (Success criteria có measurable không?)
   - **Scenario Coverage** (Tất cả flows/cases có được address không?)
   - **Edge Case Coverage** (Boundary conditions có được define không?)
   - **Non-Functional Requirements** (Performance, Security, Accessibility, etc. - có được specify không?)
   - **Dependencies & Assumptions** (Có được document và validate không?)
   - **Ambiguities & Conflicts** (Cái gì cần clarification?)

   **CÁCH VIẾT CHECKLIST ITEMS - "Unit Tests cho Tiếng Việt"**:

   ❌ **SAI** (Testing implementation):
   - "Verify landing page displays 3 episode cards"
   - "Test hover states work on desktop"
   - "Confirm logo click navigates home"

   ✅ **ĐÚNG** (Testing requirements quality):
   - "Are the exact number and layout of featured episodes specified?" [Completeness]
   - "Is 'prominent display' quantified with specific sizing/positioning?" [Clarity]
   - "Are hover state requirements consistent across all interactive elements?" [Consistency]
   - "Are keyboard navigation requirements defined for all interactive UI?" [Coverage]
   - "Is the fallback behavior specified when logo image fails to load?" [Edge Cases]
   - "Are loading states defined for asynchronous episode data?" [Completeness]
   - "Does the spec define visual hierarchy for competing UI elements?" [Clarity]

   **ITEM STRUCTURE**:
   Mỗi item nên follow pattern này:
   - Question format hỏi về requirement quality
   - Focus vào cái đã ĐƯỢC VIẾT (hoặc chưa được viết) trong spec/plan
   - Include quality dimension trong brackets [Completeness/Clarity/Consistency/etc.]
   - Reference spec section `[Spec §X.Y]` khi check existing requirements
   - Dùng `[Gap]` marker khi check missing requirements

   **VÍ DỤ THEO QUALITY DIMENSION**:

   Completeness:
   - "Are error handling requirements defined for all API failure modes? [Gap]"
   - "Are accessibility requirements specified for all interactive elements? [Completeness]"
   - "Are mobile breakpoint requirements defined for responsive layouts? [Gap]"

   Clarity:
   - "Is 'fast loading' quantified with specific timing thresholds? [Clarity, Spec §NFR-2]"
   - "Are 'related episodes' selection criteria explicitly defined? [Clarity, Spec §FR-5]"
   - "Is 'prominent' defined with measurable visual properties? [Ambiguity, Spec §FR-4]"

   Consistency:
   - "Do navigation requirements align across all pages? [Consistency, Spec §FR-10]"
   - "Are card component requirements consistent between landing and detail pages? [Consistency]"

   Coverage:
   - "Are requirements defined for zero-state scenarios (no episodes)? [Coverage, Edge Case]"
   - "Are concurrent user interaction scenarios addressed? [Coverage, Gap]"
   - "Are requirements specified for partial data loading failures? [Coverage, Exception Flow]"

   Measurability:
   - "Are visual hierarchy requirements measurable/testable? [Acceptance Criteria, Spec §FR-1]"
   - "Can 'balanced visual weight' be objectively verified? [Measurability, Spec §FR-2]"

   **Scenario Classification & Coverage** (Requirements Quality Focus):
   - Check nếu requirements exist cho: Primary, Alternate, Exception/Error, Recovery, Non-Functional scenarios
   - Với mỗi scenario class, hỏi: "Are [scenario type] requirements complete, clear, and consistent?"
   - Nếu scenario class missing: "Are [scenario type] requirements intentionally excluded or missing? [Gap]"
   - Include resilience/rollback khi state mutation occurs: "Are rollback requirements defined for migration failures? [Gap]"

   **Traceability Requirements**:
   - MINIMUM: ≥80% của items PHẢI include ít nhất một traceability reference
   - Mỗi item nên reference: spec section `[Spec §X.Y]`, hoặc dùng markers: `[Gap]`, `[Ambiguity]`, `[Conflict]`, `[Assumption]`
   - Nếu không có ID system: "Is a requirement & acceptance criteria ID scheme established? [Traceability]"

   **Surface & Resolve Issues** (Requirements Quality Problems):
   Hỏi questions về chính requirements:
   - Ambiguities: "Is the term 'fast' quantified with specific metrics? [Ambiguity, Spec §NFR-1]"
   - Conflicts: "Do navigation requirements conflict between §FR-10 and §FR-10a? [Conflict]"
   - Assumptions: "Is the assumption of 'always available podcast API' validated? [Assumption]"
   - Dependencies: "Are external podcast API requirements documented? [Dependency, Gap]"
   - Missing definitions: "Is 'visual hierarchy' defined with measurable criteria? [Gap]"

   **Content Consolidation**:
   - Soft cap: Nếu raw candidate items > 40, prioritize theo risk/impact
   - Merge near-duplicates check cùng requirement aspect
   - Nếu >5 low-impact edge cases, tạo một item: "Are edge cases X, Y, Z addressed in requirements? [Coverage]"

   **🚫 TUYỆT ĐỐI CẤM** - Những cái này làm nó thành implementation test, không phải requirements test:
   - ❌ Bất kỳ item nào bắt đầu với "Verify", "Test", "Confirm", "Check" + implementation behavior
   - ❌ References tới code execution, user actions, system behavior
   - ❌ "Displays correctly", "works properly", "functions as expected"
   - ❌ "Click", "navigate", "render", "load", "execute"
   - ❌ Test cases, test plans, QA procedures
   - ❌ Implementation details (frameworks, APIs, algorithms)

   **✅ PATTERNS BẮT BUỘC** - Những cái này test requirements quality:
   - ✅ "Are [requirement type] defined/specified/documented for [scenario]?"
   - ✅ "Is [vague term] quantified/clarified with specific criteria?"
   - ✅ "Are requirements consistent between [section A] and [section B]?"
   - ✅ "Can [requirement] be objectively measured/verified?"
   - ✅ "Are [edge cases/scenarios] addressed in requirements?"
   - ✅ "Does the spec define [missing aspect]?"

6. **Structure Reference**: Generate checklist follow canonical template trong `.specify/templates/checklist-template.md` cho title, meta section, category headings, và ID formatting. Nếu template không available, dùng: H1 title, purpose/created meta lines, `##` category sections chứa `- [ ] CHK### <requirement item>` lines với globally incrementing IDs bắt đầu từ CHK001.

7. **Report**: Output full path tới checklist đã tạo, item count, và remind user rằng mỗi run tạo một file mới. Summarize:
   - Focus areas đã chọn
   - Depth level
   - Actor/timing
   - Bất kỳ explicit user-specified must-have items nào đã được incorporate

**Quan trọng**: Mỗi lần invoke `/speckit.checklist` command tạo một checklist file dùng short, descriptive names trừ khi file đã tồn tại. Điều này cho phép:

- Nhiều checklists của các types khác nhau (ví dụ: `ux.md`, `test.md`, `security.md`)
- Simple, memorable filenames indicate checklist purpose
- Easy identification và navigation trong `checklists/` folder

Để tránh clutter, dùng descriptive types và clean up obsolete checklists khi done.

## Các loại Checklist Ví dụ & Sample Items

**UX Requirements Quality:** `ux.md`

Sample items (testing requirements, KHÔNG phải implementation):

- "Are visual hierarchy requirements defined with measurable criteria? [Clarity, Spec §FR-1]"
- "Is the number and positioning of UI elements explicitly specified? [Completeness, Spec §FR-1]"
- "Are interaction state requirements (hover, focus, active) consistently defined? [Consistency]"
- "Are accessibility requirements specified for all interactive elements? [Coverage, Gap]"
- "Is fallback behavior defined when images fail to load? [Edge Case, Gap]"
- "Can 'prominent display' be objectively measured? [Measurability, Spec §FR-4]"

**API Requirements Quality:** `api.md`

Sample items:

- "Are error response formats specified for all failure scenarios? [Completeness]"
- "Are rate limiting requirements quantified with specific thresholds? [Clarity]"
- "Are authentication requirements consistent across all endpoints? [Consistency]"
- "Are retry/timeout requirements defined for external dependencies? [Coverage, Gap]"
- "Is versioning strategy documented in requirements? [Gap]"

**Performance Requirements Quality:** `performance.md`

Sample items:

- "Are performance requirements quantified with specific metrics? [Clarity]"
- "Are performance targets defined for all critical user journeys? [Coverage]"
- "Are performance requirements under different load conditions specified? [Completeness]"
- "Can performance requirements be objectively measured? [Measurability]"
- "Are degradation requirements defined for high-load scenarios? [Edge Case, Gap]"

**Security Requirements Quality:** `security.md`

Sample items:

- "Are authentication requirements specified for all protected resources? [Coverage]"
- "Are data protection requirements defined for sensitive information? [Completeness]"
- "Is the threat model documented and requirements aligned to it? [Traceability]"
- "Are security requirements consistent with compliance obligations? [Consistency]"
- "Are security failure/breach response requirements defined? [Gap, Exception Flow]"

## Anti-Examples: Cái gì KHÔNG được làm

**❌ SAI - Những cái này test implementation, không phải requirements:**

```markdown
- [ ] CHK001 - Verify landing page displays 3 episode cards [Spec §FR-001]
- [ ] CHK002 - Test hover states work correctly on desktop [Spec §FR-003]
- [ ] CHK003 - Confirm logo click navigates to home page [Spec §FR-010]
- [ ] CHK004 - Check that related episodes section shows 3-5 items [Spec §FR-005]
```

**✅ ĐÚNG - Những cái này test requirements quality:**

```markdown
- [ ] CHK001 - Are the number and layout of featured episodes explicitly specified? [Completeness, Spec §FR-001]
- [ ] CHK002 - Are hover state requirements consistently defined for all interactive elements? [Consistency, Spec §FR-003]
- [ ] CHK003 - Are navigation requirements clear for all clickable brand elements? [Clarity, Spec §FR-010]
- [ ] CHK004 - Is the selection criteria for related episodes documented? [Gap, Spec §FR-005]
- [ ] CHK005 - Are loading state requirements defined for asynchronous episode data? [Gap]
- [ ] CHK006 - Can "visual hierarchy" requirements be objectively measured? [Measurability, Spec §FR-001]
```

**Sự khác biệt chính**:

- Sai: Test xem system có hoạt động đúng không
- Đúng: Test xem requirements có được viết đúng không
- Sai: Verification of behavior
- Đúng: Validation of requirement quality
- Sai: "Does it do X?"
- Đúng: "Is X clearly specified?"
