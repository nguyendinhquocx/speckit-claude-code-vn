---
description: "Template danh sách tasks cho triển khai tính năng"
---

# Tasks: [TÊN TÍNH NĂNG]

**Input**: Tài liệu thiết kế từ `/specs/[###-feature-name]/`
**Prerequisites**: plan.md (bắt buộc), spec.md (bắt buộc cho user stories), research.md, data-model.md, contracts/

**Tests**: Các ví dụ bên dưới bao gồm test tasks. Tests là TÙY CHỌN - chỉ bao gồm nếu được yêu cầu rõ ràng trong đặc tả tính năng.

**Tổ chức**: Tasks được nhóm theo user story để cho phép triển khai và kiểm thử độc lập từng story.

## Format: `[ID] [P?] [Story] Mô tả`

- **[P]**: Có thể chạy song song (parallel) - files khác nhau, không phụ thuộc
- **[Story]**: Task thuộc user story nào (ví dụ: US1, US2, US3)
- Bao gồm đường dẫn file chính xác trong mô tả

## Quy ước Đường dẫn

- **Single project**: `src/`, `tests/` ở repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` hoặc `android/src/`
- Đường dẫn dưới đây giả định single project - điều chỉnh dựa trên cấu trúc plan.md

<!--
  ============================================================================
  QUAN TRỌNG: Các tasks bên dưới là TASKS MẪU chỉ để minh họa.

  Lệnh /speckit.tasks PHẢI thay thế chúng bằng tasks thực tế dựa trên:
  - User stories từ spec.md (với priorities P1, P2, P3...)
  - Yêu cầu tính năng từ plan.md
  - Entities từ data-model.md
  - Endpoints từ contracts/

  Tasks PHẢI được tổ chức theo user story để mỗi story có thể:
  - Triển khai độc lập
  - Kiểm thử độc lập
  - Giao hàng như một MVP increment

  KHÔNG giữ lại các sample tasks này trong file tasks.md được generate.
  ============================================================================
-->

## Phase 1: Setup (Hạ tầng Chung)

**Mục đích**: Khởi tạo project và cấu trúc cơ bản

- [ ] T001 Tạo cấu trúc project theo implementation plan
- [ ] T002 Khởi tạo project [language] với dependencies [framework]
- [ ] T003 [P] Cấu hình linting và formatting tools

---

## Phase 2: Foundational (Prerequisites Chặn)

**Mục đích**: Hạ tầng lõi PHẢI hoàn thành trước KHI BẤT KỲ user story nào được triển khai

**⚠️ CRITICAL**: Không thể bắt đầu làm user story cho đến khi phase này hoàn thành

Ví dụ các foundational tasks (điều chỉnh dựa trên project của bạn):

- [ ] T004 Setup database schema và migrations framework
- [ ] T005 [P] Implement authentication/authorization framework
- [ ] T006 [P] Setup API routing và middleware structure
- [ ] T007 Tạo base models/entities mà tất cả stories phụ thuộc
- [ ] T008 Cấu hình error handling và logging infrastructure
- [ ] T009 Setup environment configuration management

**Checkpoint**: Foundation sẵn sàng - user story implementation giờ có thể bắt đầu song song

---

## Phase 3: User Story 1 - [Tiêu đề] (Ưu tiên: P1) 🎯 MVP

**Mục tiêu**: [Mô tả ngắn gọn story này mang lại gì]

**Kiểm thử độc lập**: [Cách xác minh story này hoạt động độc lập]

### Tests cho User Story 1 (TÙY CHỌN - chỉ khi tests được yêu cầu) ⚠️

> **LƯU Ý: Viết tests này TRƯỚC, đảm bảo chúng FAIL trước khi implementation**

- [ ] T010 [P] [US1] Contract test cho [endpoint] trong tests/contract/test_[name].py
- [ ] T011 [P] [US1] Integration test cho [user journey] trong tests/integration/test_[name].py

### Implementation cho User Story 1

- [ ] T012 [P] [US1] Tạo [Entity1] model trong src/models/[entity1].py
- [ ] T013 [P] [US1] Tạo [Entity2] model trong src/models/[entity2].py
- [ ] T014 [US1] Implement [Service] trong src/services/[service].py (phụ thuộc T012, T013)
- [ ] T015 [US1] Implement [endpoint/feature] trong src/[location]/[file].py
- [ ] T016 [US1] Thêm validation và error handling
- [ ] T017 [US1] Thêm logging cho user story 1 operations

**Checkpoint**: Tại điểm này, User Story 1 phải fully functional và testable độc lập

---

## Phase 4: User Story 2 - [Tiêu đề] (Ưu tiên: P2)

**Mục tiêu**: [Mô tả ngắn gọn story này mang lại gì]

**Kiểm thử độc lập**: [Cách xác minh story này hoạt động độc lập]

### Tests cho User Story 2 (TÙY CHỌN - chỉ khi tests được yêu cầu) ⚠️

- [ ] T018 [P] [US2] Contract test cho [endpoint] trong tests/contract/test_[name].py
- [ ] T019 [P] [US2] Integration test cho [user journey] trong tests/integration/test_[name].py

### Implementation cho User Story 2

- [ ] T020 [P] [US2] Tạo [Entity] model trong src/models/[entity].py
- [ ] T021 [US2] Implement [Service] trong src/services/[service].py
- [ ] T022 [US2] Implement [endpoint/feature] trong src/[location]/[file].py
- [ ] T023 [US2] Tích hợp với User Story 1 components (nếu cần)

**Checkpoint**: Tại điểm này, User Stories 1 VÀ 2 phải đều hoạt động độc lập

---

## Phase 5: User Story 3 - [Tiêu đề] (Ưu tiên: P3)

**Mục tiêu**: [Mô tả ngắn gọn story này mang lại gì]

**Kiểm thử độc lập**: [Cách xác minh story này hoạt động độc lập]

### Tests cho User Story 3 (TÙY CHỌN - chỉ khi tests được yêu cầu) ⚠️

- [ ] T024 [P] [US3] Contract test cho [endpoint] trong tests/contract/test_[name].py
- [ ] T025 [P] [US3] Integration test cho [user journey] trong tests/integration/test_[name].py

### Implementation cho User Story 3

- [ ] T026 [P] [US3] Tạo [Entity] model trong src/models/[entity].py
- [ ] T027 [US3] Implement [Service] trong src/services/[service].py
- [ ] T028 [US3] Implement [endpoint/feature] trong src/[location]/[file].py

**Checkpoint**: Tất cả user stories giờ phải independently functional

---

[Thêm các user story phases khác nếu cần, theo cùng pattern]

---

## Phase N: Polish & Cross-Cutting Concerns

**Mục đích**: Cải tiến ảnh hưởng đến nhiều user stories

- [ ] TXXX [P] Cập nhật documentation trong docs/
- [ ] TXXX Code cleanup và refactoring
- [ ] TXXX Performance optimization across all stories
- [ ] TXXX [P] Additional unit tests (nếu được yêu cầu) trong tests/unit/
- [ ] TXXX Security hardening
- [ ] TXXX Chạy validation quickstart.md

---

## Phụ thuộc & Thứ tự Thực thi

### Phụ thuộc Phase

- **Setup (Phase 1)**: Không có phụ thuộc - có thể bắt đầu ngay
- **Foundational (Phase 2)**: Phụ thuộc Setup hoàn thành - CHẶN tất cả user stories
- **User Stories (Phase 3+)**: Tất cả phụ thuộc Foundational phase hoàn thành
  - User stories sau đó có thể tiến hành song song (nếu có nhân lực)
  - Hoặc tuần tự theo thứ tự ưu tiên (P1 → P2 → P3)
- **Polish (Final Phase)**: Phụ thuộc tất cả user stories mong muốn hoàn thành

### Phụ thuộc User Story

- **User Story 1 (P1)**: Có thể bắt đầu sau Foundational (Phase 2) - Không phụ thuộc stories khác
- **User Story 2 (P2)**: Có thể bắt đầu sau Foundational (Phase 2) - Có thể tích hợp với US1 nhưng phải testable độc lập
- **User Story 3 (P3)**: Có thể bắt đầu sau Foundational (Phase 2) - Có thể tích hợp với US1/US2 nhưng phải testable độc lập

### Trong Mỗi User Story

- Tests (nếu có) PHẢI được viết và FAIL trước implementation
- Models trước services
- Services trước endpoints
- Core implementation trước integration
- Story hoàn thành trước khi chuyển sang priority tiếp theo

### Cơ hội Song song

- Tất cả Setup tasks có đánh dấu [P] có thể chạy song song
- Tất cả Foundational tasks có đánh dấu [P] có thể chạy song song (trong Phase 2)
- Khi Foundational phase hoàn thành, tất cả user stories có thể bắt đầu song song (nếu team capacity cho phép)
- Tất cả tests cho một user story có đánh dấu [P] có thể chạy song song
- Models trong một story có đánh dấu [P] có thể chạy song song
- Các user stories khác nhau có thể được làm song song bởi team members khác nhau

---

## Ví dụ Parallel: User Story 1

```bash
# Launch tất cả tests cho User Story 1 cùng lúc (nếu tests được yêu cầu):
Task: "Contract test cho [endpoint] trong tests/contract/test_[name].py"
Task: "Integration test cho [user journey] trong tests/integration/test_[name].py"

# Launch tất cả models cho User Story 1 cùng lúc:
Task: "Tạo [Entity1] model trong src/models/[entity1].py"
Task: "Tạo [Entity2] model trong src/models/[entity2].py"
```

---

## Chiến lược Triển khai

### MVP First (Chỉ User Story 1)

1. Hoàn thành Phase 1: Setup
2. Hoàn thành Phase 2: Foundational (CRITICAL - chặn tất cả stories)
3. Hoàn thành Phase 3: User Story 1
4. **DỪNG và VALIDATE**: Test User Story 1 độc lập
5. Deploy/demo nếu sẵn sàng

### Incremental Delivery

1. Hoàn thành Setup + Foundational → Foundation sẵn sàng
2. Thêm User Story 1 → Test độc lập → Deploy/Demo (MVP!)
3. Thêm User Story 2 → Test độc lập → Deploy/Demo
4. Thêm User Story 3 → Test độc lập → Deploy/Demo
5. Mỗi story thêm value mà không phá vỡ stories trước đó

### Chiến lược Parallel Team

Với nhiều developers:

1. Team hoàn thành Setup + Foundational cùng nhau
2. Khi Foundational xong:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Stories hoàn thành và tích hợp độc lập

---

## Lưu ý

- [P] tasks = files khác nhau, không có phụ thuộc
- [Story] label map task với user story cụ thể để traceability
- Mỗi user story phải có thể completable và testable độc lập
- Verify tests fail trước khi implementing
- Commit sau mỗi task hoặc logical group
- Dừng tại bất kỳ checkpoint nào để validate story độc lập
- Tránh: vague tasks, same file conflicts, cross-story dependencies làm phá vỡ independence

---

## Hướng dẫn cho AI Generation (XÓA section này sau khi generate tasks.md thực tế)

### Nguyên tắc Generate Tasks

1. **Đọc spec.md TRƯỚC**: Hiểu user stories với priorities (P1, P2, P3)
2. **Đọc plan.md**: Hiểu tech stack, cấu trúc project, architecture decisions
3. **Đọc data-model.md** (nếu có): Map entities → tasks
4. **Đọc contracts/** (nếu có): Map endpoints → tasks
5. **Organize by User Story**: Mỗi story = một phase riêng biệt

### Format Tasks Bắt buộc

**EVERY task MUST follow**:

```
- [ ] T### [P?] [Story?] Mô tả với đường dẫn file cụ thể
```

**Components**:
- `- [ ]`: Markdown checkbox (bắt buộc)
- `T###`: Task ID tuần tự (T001, T002, T003...)
- `[P]`: OPTIONAL - chỉ nếu task có thể chạy song song
- `[Story]`: OPTIONAL cho Setup/Foundational/Polish, BẮT BUỘC cho user story tasks (US1, US2, US3)
- Mô tả: Action rõ ràng + đường dẫn file CHÍNH XÁC

**Ví dụ ĐÚNG**:
- `- [ ] T001 Tạo cấu trúc project theo implementation plan`
- `- [ ] T012 [P] [US1] Tạo User model trong src/models/user.py`
- `- [ ] T014 [US1] Implement UserService trong src/services/user_service.py`

**Ví dụ SAI**:
- `T001 Tạo project` (thiếu checkbox, thiếu đường dẫn)
- `- [ ] Tạo User model` (thiếu ID, thiếu story label, thiếu path)
- `- [ ] [US1] Tạo model` (thiếu Task ID, thiếu path)

### Mapping Logic

**Từ spec.md User Stories → Phases**:
- P1 story → Phase 3
- P2 story → Phase 4
- P3 story → Phase 5
- etc.

**Từ data-model.md Entities → Tasks**:
- Mỗi entity → task "Tạo [Entity] model trong src/models/[entity].py"
- Map entity về user story cần nó

**Từ contracts/ Endpoints → Tasks**:
- Mỗi endpoint → task "Implement [endpoint] trong src/api/[file].py"
- Nếu tests requested → thêm contract test task TRƯỚC implementation

**Từ plan.md Tech Stack → Setup Tasks**:
- Language/Framework → khởi tạo project
- Storage → setup database migrations
- Testing → cấu hình test framework

### Validation Trước khi Output

- ✅ Tất cả tasks follow format đúng?
- ✅ Mỗi user story có phase riêng?
- ✅ Tasks organized P1 → P2 → P3?
- ✅ Foundational phase có đủ để unblock stories?
- ✅ Mỗi task có đường dẫn file cụ thể?
- ✅ [P] markers chỉ cho tasks thật sự parallelizable?
- ✅ [Story] labels consistent với spec.md?

Nếu có "KHÔNG" → Sửa trước khi output.
