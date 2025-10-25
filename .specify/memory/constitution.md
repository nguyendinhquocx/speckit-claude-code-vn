# [PROJECT_NAME] Constitution
<!-- Ví dụ: Spec Constitution, TaskFlow Constitution, etc. -->

## Nguyên tắc Cốt lõi

### [PRINCIPLE_1_NAME]
<!-- Ví dụ: I. Library-First -->
[PRINCIPLE_1_DESCRIPTION]
<!-- Ví dụ: Mọi tính năng bắt đầu như standalone library; Libraries phải self-contained, independently testable, documented; Clear purpose required - không có organizational-only libraries -->

### [PRINCIPLE_2_NAME]
<!-- Ví dụ: II. CLI Interface -->
[PRINCIPLE_2_DESCRIPTION]
<!-- Ví dụ: Mọi library expose functionality via CLI; Text in/out protocol: stdin/args → stdout, errors → stderr; Support JSON + human-readable formats -->

### [PRINCIPLE_3_NAME]
<!-- Ví dụ: III. Test-First (NON-NEGOTIABLE) -->
[PRINCIPLE_3_DESCRIPTION]
<!-- Ví dụ: TDD bắt buộc: Tests written → User approved → Tests fail → Then implement; Red-Green-Refactor cycle strictly enforced -->

### [PRINCIPLE_4_NAME]
<!-- Ví dụ: IV. Integration Testing -->
[PRINCIPLE_4_DESCRIPTION]
<!-- Ví dụ: Focus areas requiring integration tests: New library contract tests, Contract changes, Inter-service communication, Shared schemas -->

### [PRINCIPLE_5_NAME]
<!-- Ví dụ: V. Observability, VI. Versioning & Breaking Changes, VII. Simplicity -->
[PRINCIPLE_5_DESCRIPTION]
<!-- Ví dụ: Text I/O ensures debuggability; Structured logging required; Hoặc: MAJOR.MINOR.BUILD format; Hoặc: Bắt đầu đơn giản, YAGNI principles -->

## [SECTION_2_NAME]
<!-- Ví dụ: Additional Constraints, Security Requirements, Performance Standards, etc. -->

[SECTION_2_CONTENT]
<!-- Ví dụ: Technology stack requirements, compliance standards, deployment policies, etc. -->

## [SECTION_3_NAME]
<!-- Ví dụ: Development Workflow, Review Process, Quality Gates, etc. -->

[SECTION_3_CONTENT]
<!-- Ví dụ: Code review requirements, testing gates, deployment approval process, etc. -->

## Quản trị
<!-- Ví dụ: Constitution supersedes tất cả practices khác; Amendments require documentation, approval, migration plan -->

[GOVERNANCE_RULES]
<!-- Ví dụ: Tất cả PRs/reviews phải verify compliance; Complexity phải được justified; Dùng [GUIDANCE_FILE] cho runtime development guidance -->

**Version**: [CONSTITUTION_VERSION] | **Ratified**: [RATIFICATION_DATE] | **Last Amended**: [LAST_AMENDED_DATE]
<!-- Ví dụ: Version: 2.1.1 | Ratified: 2025-06-13 | Last Amended: 2025-07-16 -->
