---
description: Thực hiện phân tích cross-artifact consistency và quality analysis non-destructive across spec.md, plan.md, và tasks.md sau task generation.
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Mục tiêu

Identify inconsistencies, duplications, ambiguities, và underspecified items across ba core artifacts (`spec.md`, `plan.md`, `tasks.md`) trước implementation. Command này PHẢI chạy chỉ sau khi `/speckit.tasks` đã successfully produced complete `tasks.md`.

## Operating Constraints

**STRICTLY READ-ONLY**: **Không** modify bất kỳ files nào. Output structured analysis report. Offer optional remediation plan (user phải explicitly approve trước khi bất kỳ follow-up editing commands nào được invoked manually).

**Constitution Authority**: Project constitution (`.specify/memory/constitution.md`) là **non-negotiable** trong analysis scope này. Constitution conflicts tự động là CRITICAL và require adjustment của spec, plan, hoặc tasks—không phải dilution, reinterpretation, hoặc silent ignoring của principle. Nếu một principle itself cần change, điều đó phải occur trong separate, explicit constitution update bên ngoài `/speckit.analyze`.

## Execution Steps

### 1. Khởi tạo Analysis Context

Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` một lần từ repo root và parse JSON cho FEATURE_DIR và AVAILABLE_DOCS. Derive absolute paths:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

Abort với error message nếu bất kỳ required file nào missing (instruct user chạy missing prerequisite command).
Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot").

### 2. Load Artifacts (Progressive Disclosure)

Load chỉ minimal necessary context từ mỗi artifact:

**Từ spec.md:**
- Overview/Context
- Functional Requirements
- Non-Functional Requirements
- User Stories
- Edge Cases (nếu present)

**Từ plan.md:**
- Architecture/stack choices
- Data Model references
- Phases
- Technical constraints

**Từ tasks.md:**
- Task IDs
- Descriptions
- Phase grouping
- Parallel markers [P]
- Referenced file paths

**Từ constitution:**
- Load `.specify/memory/constitution.md` cho principle validation

### 3. Build Semantic Models

Tạo internal representations (không include raw artifacts trong output):

- **Requirements inventory**: Mỗi functional + non-functional requirement với stable key
- **User story/action inventory**: Discrete user actions với acceptance criteria
- **Task coverage mapping**: Map mỗi task tới một hoặc nhiều requirements hoặc stories
- **Constitution rule set**: Extract principle names và MUST/SHOULD normative statements

### 4. Detection Passes (Token-Efficient Analysis)

Focus vào high-signal findings. Limit tới 50 findings total; aggregate remainder trong overflow summary.

#### A. Duplication Detection
- Identify near-duplicate requirements
- Đánh dấu lower-quality phrasing cho consolidation

#### B. Ambiguity Detection
- Flag vague adjectives (fast, scalable, secure, intuitive, robust) thiếu measurable criteria
- Flag unresolved placeholders (TODO, TKTK, ???, `<placeholder>`, etc.)

#### C. Underspecification
- Requirements với verbs nhưng thiếu object hoặc measurable outcome
- User stories thiếu acceptance criteria alignment
- Tasks referencing files hoặc components không defined trong spec/plan

#### D. Constitution Alignment
- Bất kỳ requirement hoặc plan element nào conflicting với MUST principle
- Thiếu mandated sections hoặc quality gates từ constitution

#### E. Coverage Gaps
- Requirements với zero associated tasks
- Tasks với no mapped requirement/story
- Non-functional requirements không reflected trong tasks (ví dụ: performance, security)

#### F. Inconsistency
- Terminology drift (same concept named differently across files)
- Data entities referenced trong plan nhưng absent trong spec (hoặc vice versa)
- Task ordering contradictions (ví dụ: integration tasks trước foundational setup tasks mà không có dependency note)
- Conflicting requirements (ví dụ: một requires Next.js trong khi khác specifies Vue)

### 5. Severity Assignment

Dùng heuristic này để prioritize findings:

- **CRITICAL**: Vi phạm constitution MUST, thiếu core spec artifact, hoặc requirement với zero coverage blocks baseline functionality
- **HIGH**: Duplicate hoặc conflicting requirement, ambiguous security/performance attribute, untestable acceptance criterion
- **MEDIUM**: Terminology drift, thiếu non-functional task coverage, underspecified edge case
- **LOW**: Style/wording improvements, minor redundancy không affecting execution order

### 6. Tạo Compact Analysis Report

Output Markdown report (no file writes) với structure sau:

## Specification Analysis Report

| ID | Category | Severity | Location(s) | Tóm tắt | Khuyến nghị |
|----|----------|----------|-------------|---------|-------------|
| A1 | Duplication | HIGH | spec.md:L120-134 | Hai similar requirements ... | Merge phrasing; giữ clearer version |

(Thêm một row per finding; generate stable IDs prefixed by category initial.)

**Coverage Summary Table:**

| Requirement Key | Có Task? | Task IDs | Ghi chú |
|-----------------|----------|----------|---------|

**Constitution Alignment Issues:** (nếu có)

**Unmapped Tasks:** (nếu có)

**Metrics:**

- Total Requirements
- Total Tasks
- Coverage % (requirements với >=1 task)
- Ambiguity Count
- Duplication Count
- Critical Issues Count

### 7. Provide Next Actions

Ở cuối report, output concise Next Actions block:

- Nếu CRITICAL issues exist: Recommend resolving trước `/speckit.implement`
- Nếu chỉ LOW/MEDIUM: User có thể proceed, nhưng provide improvement suggestions
- Provide explicit command suggestions

### 8. Offer Remediation

Hỏi user: "Mày có muốn tao suggest concrete remediation edits cho top N issues không?" (KHÔNG apply chúng automatically.)

## Operating Principles

### Context Efficiency
- **Minimal high-signal tokens**: Focus vào actionable findings, không phải exhaustive documentation
- **Progressive disclosure**: Load artifacts incrementally
- **Token-efficient output**: Limit findings table tới 50 rows
- **Deterministic results**: Rerunning mà không có changes phải produce consistent IDs và counts

### Analysis Guidelines
- **KHÔNG BAO GIỜ modify files** (đây là read-only analysis)
- **KHÔNG BAO GIỜ hallucinate missing sections**
- **Prioritize constitution violations** (chúng luôn CRITICAL)
- **Dùng examples over exhaustive rules**
- **Report zero issues gracefully**

## Context

$ARGUMENTS
