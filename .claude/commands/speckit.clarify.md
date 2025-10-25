---
description: Identify underspecified areas trong feature spec hiện tại bằng cách hỏi tối đa 5 câu hỏi làm rõ highly targeted và encode answers trở lại vào spec.
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Outline

Mục tiêu: Detect và reduce ambiguity hoặc missing decision points trong active feature specification và ghi lại clarifications trực tiếp trong spec file.

Lưu ý: Clarification workflow này expected là chạy (và hoàn thành) TRƯỚC KHI invoke `/speckit.plan`. Nếu user explicitly nói họ skip clarification (ví dụ: exploratory spike), mày có thể proceed, nhưng phải warn rằng downstream rework risk tăng lên.

Các bước thực thi:

1. Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly` từ repo root **một lần** (combined `--json --paths-only` mode / `-Json -PathsOnly`). Parse minimal JSON payload fields:
   - `FEATURE_DIR`
   - `FEATURE_SPEC`
   - (Optionally capture `IMPL_PLAN`, `TASKS` cho future chained flows.)
   - Nếu JSON parsing fails, abort và instruct user chạy lại `/speckit.specify` hoặc verify feature branch environment.
   - Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

2. Load current spec file. Thực hiện structured ambiguity & coverage scan sử dụng taxonomy này. Với mỗi category, đánh dấu status: Clear / Partial / Missing. Tạo internal coverage map dùng cho prioritization (không output raw map trừ khi không có câu hỏi nào sẽ được hỏi).

   Functional Scope & Behavior:
   - Core user goals & success criteria
   - Explicit out-of-scope declarations
   - User roles / personas differentiation

   Domain & Data Model:
   - Entities, attributes, relationships
   - Identity & uniqueness rules
   - Lifecycle/state transitions
   - Data volume / scale assumptions

   Interaction & UX Flow:
   - Critical user journeys / sequences
   - Error/empty/loading states
   - Accessibility hoặc localization notes

   Non-Functional Quality Attributes:
   - Performance (latency, throughput targets)
   - Scalability (horizontal/vertical, limits)
   - Reliability & availability (uptime, recovery expectations)
   - Observability (logging, metrics, tracing signals)
   - Security & privacy (authN/Z, data protection, threat assumptions)
   - Compliance / regulatory constraints (nếu có)

   Integration & External Dependencies:
   - External services/APIs và failure modes
   - Data import/export formats
   - Protocol/versioning assumptions

   Edge Cases & Failure Handling:
   - Negative scenarios
   - Rate limiting / throttling
   - Conflict resolution (ví dụ: concurrent edits)

   Constraints & Tradeoffs:
   - Technical constraints (language, storage, hosting)
   - Explicit tradeoffs hoặc rejected alternatives

   Terminology & Consistency:
   - Canonical glossary terms
   - Avoided synonyms / deprecated terms

   Completion Signals:
   - Acceptance criteria testability
   - Measurable Definition of Done style indicators

   Misc / Placeholders:
   - TODO markers / unresolved decisions
   - Ambiguous adjectives ("robust", "intuitive") thiếu quantification

   Với mỗi category với Partial hoặc Missing status, thêm candidate question opportunity trừ khi:
   - Clarification sẽ không materially change implementation hoặc validation strategy
   - Information tốt hơn là deferred sang planning phase (note internally)

3. Generate (internally) một prioritized queue của candidate clarification questions (maximum 5). KHÔNG output tất cả một lúc. Áp dụng các constraints này:
    - Maximum 10 total questions across toàn bộ session.
    - Mỗi câu hỏi phải có thể trả lời bằng HOẶC:
       - Một short multiple‑choice selection (2–5 distinct, mutually exclusive options), HOẶC
       - Một one-word / short‑phrase answer (explicitly constrain: "Trả lời trong <=5 từ").
    - Chỉ bao gồm questions mà answers của chúng materially impact architecture, data modeling, task decomposition, test design, UX behavior, operational readiness, hoặc compliance validation.
    - Đảm bảo category coverage balance: cố gắng cover highest impact unresolved categories trước; tránh hỏi hai low-impact questions khi một single high-impact area (ví dụ: security posture) chưa resolved.
    - Exclude questions đã answered, trivial stylistic preferences, hoặc plan-level execution details (trừ khi blocking correctness).
    - Ưu tiên clarifications reduce downstream rework risk hoặc prevent misaligned acceptance tests.
    - Nếu hơn 5 categories remain unresolved, chọn top 5 theo (Impact * Uncertainty) heuristic.

4. Sequential questioning loop (interactive):
    - Present CHÍNH XÁC MỘT câu hỏi tại một thời điểm.
    - Với multiple‑choice questions:
       - **Analyze tất cả options** và xác định **most suitable option** dựa trên:
          - Best practices cho project type
          - Common patterns trong similar implementations
          - Risk reduction (security, performance, maintainability)
          - Alignment với bất kỳ explicit project goals hoặc constraints visible trong spec
       - Present **recommended option prominently** ở top với clear reasoning (1-2 câu giải thích tại sao đây là best choice).
       - Format như: `**Khuyến nghị:** Option [X] - <lý do>`
       - Sau đó render tất cả options như Markdown table:

       | Option | Mô tả |
       |--------|-------|
       | A | <Mô tả Option A> |
       | B | <Mô tả Option B> |
       | C | <Mô tả Option C> (thêm D/E nếu cần up to 5) |
       | Short | Cung cấp câu trả lời ngắn khác (<=5 từ) (Chỉ bao gồm nếu free-form alternative phù hợp) |

       - Sau table, thêm: `Mày có thể reply với option letter (ví dụ: "A"), chấp nhận recommendation bằng cách nói "yes" hoặc "recommended", hoặc cung cấp câu trả lời ngắn của riêng mày.`
    - Với short‑answer style (không có meaningful discrete options):
       - Provide **suggested answer** của mày dựa trên best practices và context.
       - Format như: `**Gợi ý:** <câu trả lời đề xuất của mày> - <lý do ngắn gọn>`
       - Sau đó output: `Format: Câu trả lời ngắn (<=5 từ). Mày có thể chấp nhận suggestion bằng cách nói "yes" hoặc "suggested", hoặc cung cấp câu trả lời của riêng mày.`
    - Sau khi user answers:
       - Nếu user replies với "yes", "recommended", hoặc "suggested", dùng previously stated recommendation/suggestion của mày làm answer.
       - Otherwise, validate answer maps tới one option hoặc fits <=5 word constraint.
       - Nếu ambiguous, hỏi quick disambiguation (count vẫn thuộc về same question; không advance).
       - Khi satisfactory, record nó trong working memory (chưa write to disk) và move tới next queued question.
    - Dừng hỏi further questions khi:
       - Tất cả critical ambiguities resolved early (remaining queued items trở nên unnecessary), HOẶC
       - User signals completion ("done", "good", "no more"), HOẶC
       - Mày reach 5 asked questions.
    - Không bao giờ reveal future queued questions trước.
    - Nếu không có valid questions tồn tại lúc start, ngay lập tức report no critical ambiguities.

5. Integration sau MỖI accepted answer (incremental update approach):
    - Maintain in-memory representation của spec (loaded once lúc start) plus raw file contents.
    - Với first integrated answer trong session này:
       - Đảm bảo `## Làm rõ` section tồn tại (tạo nó ngay sau highest-level contextual/overview section per spec template nếu thiếu).
       - Dưới nó, tạo (nếu chưa có) một `### Session YYYY-MM-DD` subheading cho today.
    - Append một bullet line ngay sau acceptance: `- Q: <câu hỏi> → A: <câu trả lời cuối cùng>`.

   **NGÔN NGỮ BẮT BUỘC**:
   - **Câu hỏi và câu trả lời PHẢI TIẾNG VIỆT** khi present cho user
   - Khi update spec sections, **dùng TIẾNG VIỆT** cho mọi mô tả mới thêm vào
    - Sau đó ngay lập tức apply clarification tới most appropriate section(s):
       - Functional ambiguity → Update hoặc thêm bullet trong Yêu cầu Chức năng.
       - User interaction / actor distinction → Update User Stories hoặc Actors subsection (nếu có) với clarified role, constraint, hoặc scenario.
       - Data shape / entities → Update Data Model (thêm fields, types, relationships) preserving ordering; note added constraints succinctly.
       - Non-functional constraint → Add/modify measurable criteria trong Non-Functional / Quality Attributes section (convert vague adjective thành metric hoặc explicit target).
       - Edge case / negative flow → Thêm bullet mới dưới Edge Cases / Error Handling (hoặc tạo subsection như vậy nếu template provides placeholder cho nó).
       - Terminology conflict → Normalize term across spec; retain original chỉ khi cần thiết bằng cách thêm `(formerly referred to as "X")` một lần.
    - Nếu clarification invalidates earlier ambiguous statement, thay thế statement đó instead of duplicating; không để obsolete contradictory text.
    - Save spec file SAU mỗi integration để minimize risk của context loss (atomic overwrite).
    - Preserve formatting: không reorder unrelated sections; giữ heading hierarchy intact.
    - Giữ mỗi inserted clarification minimal và testable (tránh narrative drift).

6. Validation (performed sau MỖI write plus final pass):
   - Clarifications session chứa exactly một bullet per accepted answer (no duplicates).
   - Total asked (accepted) questions ≤ 5.
   - Updated sections không chứa lingering vague placeholders mà new answer meant để resolve.
   - Không có contradictory earlier statement nào còn lại (scan cho now-invalid alternative choices removed).
   - Markdown structure valid; chỉ allowed new headings: `## Làm rõ`, `### Session YYYY-MM-DD`.
   - Terminology consistency: same canonical term used across all updated sections.

7. Write updated spec trở lại `FEATURE_SPEC`.

8. Report completion (sau questioning loop ends hoặc early termination):
   - Number của questions asked & answered.
   - Path tới updated spec.
   - Sections touched (list names).
   - Coverage summary table listing mỗi taxonomy category với Status: Resolved (was Partial/Missing và addressed), Deferred (exceeds question quota hoặc better suited cho planning), Clear (already sufficient), Outstanding (vẫn Partial/Missing nhưng low impact).
   - Nếu bất kỳ Outstanding hoặc Deferred remain, recommend có nên proceed tới `/speckit.plan` hoặc chạy `/speckit.clarify` again sau post-plan.
   - Suggested next command.

Behavior rules:

- Nếu không tìm thấy meaningful ambiguities (hoặc tất cả potential questions sẽ là low-impact), respond: "Không phát hiện critical ambiguities nào đáng để formal clarification." và suggest proceeding.
- Nếu spec file missing, instruct user chạy `/speckit.specify` trước (không tạo spec mới ở đây).
- Không bao giờ exceed 5 total asked questions (clarification retries cho một single question không count như new questions).
- Tránh speculative tech stack questions trừ khi absence blocks functional clarity.
- Respect user early termination signals ("stop", "done", "proceed").
- Nếu không có questions asked due to full coverage, output compact coverage summary (all categories Clear) sau đó suggest advancing.
- Nếu quota reached với unresolved high-impact categories remaining, explicitly flag chúng dưới Deferred với rationale.

Context cho prioritization: $ARGUMENTS
