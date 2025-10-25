# Kế hoạch Triển khai: [TÊN TÍNH NĂNG]

**Branch**: `[###-feature-name]` | **Ngày**: [DATE] | **Spec**: [link]
**Input**: Đặc tả tính năng từ `/specs/[###-feature-name]/spec.md`

**Lưu ý**: Template này được điền bởi lệnh `/speckit.plan`. Xem `.claude/commands/speckit.plan.md` để biết workflow thực thi.

## Tóm tắt

[Trích xuất từ spec: yêu cầu chính + phương pháp kỹ thuật từ research]

## Ngữ cảnh Kỹ thuật

<!--
  HÀNH ĐỘNG CẦN THIẾT: Thay thế nội dung trong section này với chi tiết kỹ thuật
  cho project. Cấu trúc ở đây được trình bày theo hướng tư vấn để hướng dẫn
  quá trình iteration.
-->

**Language/Version**: [ví dụ: Python 3.11, Swift 5.9, Rust 1.75 hoặc CẦN LÀM RÕ]
**Primary Dependencies**: [ví dụ: FastAPI, UIKit, LLVM hoặc CẦN LÀM RÕ]
**Storage**: [nếu có, ví dụ: PostgreSQL, CoreData, files hoặc N/A]
**Testing**: [ví dụ: pytest, XCTest, cargo test hoặc CẦN LÀM RÕ]
**Target Platform**: [ví dụ: Linux server, iOS 15+, WASM hoặc CẦN LÀM RÕ]
**Project Type**: [single/web/mobile - xác định cấu trúc source]
**Performance Goals**: [domain-specific, ví dụ: 1000 req/s, 10k lines/sec, 60 fps hoặc CẦN LÀM RÕ]
**Constraints**: [domain-specific, ví dụ: <200ms p95, <100MB memory, offline-capable hoặc CẦN LÀM RÕ]
**Scale/Scope**: [domain-specific, ví dụ: 10k users, 1M LOC, 50 screens hoặc CẦN LÀM RÕ]

## Kiểm tra Constitution

*GATE: Phải pass trước Phase 0 research. Kiểm tra lại sau Phase 1 design.*

[Gates được xác định dựa trên constitution file]

## Cấu trúc Project

### Documentation (tính năng này)

```text
specs/[###-feature]/
├── plan.md              # File này (output của /speckit.plan)
├── research.md          # Output Phase 0 (lệnh /speckit.plan)
├── data-model.md        # Output Phase 1 (lệnh /speckit.plan)
├── quickstart.md        # Output Phase 1 (lệnh /speckit.plan)
├── contracts/           # Output Phase 1 (lệnh /speckit.plan)
└── tasks.md             # Output Phase 2 (lệnh /speckit.tasks - KHÔNG được tạo bởi /speckit.plan)
```

### Source Code (repository root)
<!--
  HÀNH ĐỘNG CẦN THIẾT: Thay thế cây placeholder bên dưới với layout cụ thể
  cho tính năng này. Xóa các options không dùng và mở rộng cấu trúc đã chọn với
  đường dẫn thật (ví dụ: apps/admin, packages/something). Plan được giao
  không được bao gồm nhãn Option.
-->

```text
# [XÓA NẾU KHÔNG DÙNG] Option 1: Single project (MẶC ĐỊNH)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [XÓA NẾU KHÔNG DÙNG] Option 2: Web application (khi phát hiện "frontend" + "backend")
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [XÓA NẾU KHÔNG DÙNG] Option 3: Mobile + API (khi phát hiện "iOS/Android")
api/
└── [giống như backend ở trên]

ios/ hoặc android/
└── [cấu trúc platform-specific: feature modules, UI flows, platform tests]
```

**Quyết định Cấu trúc**: [Ghi lại cấu trúc đã chọn và tham chiếu đến các
thư mục thực tế được ghi lại ở trên]

## Theo dõi Độ phức tạp

> **CHỈ điền nếu Constitution Check có vi phạm cần được justify**

| Vi phạm | Tại sao Cần thiết | Phương án Đơn giản hơn Bị từ chối Vì |
|---------|-------------------|--------------------------------------|
| [ví dụ: project thứ 4] | [nhu cầu hiện tại] | [tại sao 3 projects không đủ] |
| [ví dụ: Repository pattern] | [vấn đề cụ thể] | [tại sao direct DB access không đủ] |

---

## Hướng dẫn Sử dụng Template (XÓA section này sau khi hoàn thành plan)

### Nguyên tắc Chung

- **Plan.md là tài liệu KỸ THUẬT** - khác với spec.md (business-focused)
- Ở đây MỚI được đề cập: languages, frameworks, databases, APIs, architecture
- Viết cho developers, architects, tech leads
- Phải có đủ chi tiết để generate tasks.md từ plan này

### Yêu cầu về Sections

#### Bắt buộc:
- **Tóm tắt**: Tổng quan ngắn gọn về tính năng và phương pháp kỹ thuật
- **Ngữ cảnh Kỹ thuật**: Tất cả các mục phải điền (không để trống hoặc TBD)
- **Constitution Check**: Phải kiểm tra compliance với constitution.md
- **Cấu trúc Project**: Phải chọn và ghi rõ 1 trong 3 options

#### Optional:
- **Theo dõi Độ phức tạp**: Chỉ khi có vi phạm constitution

### Cho AI Generation

Khi tạo plan từ spec.md:

1. **Đọc spec.md trước**: Hiểu WHAT và WHY trước khi quyết định HOW
2. **Đọc constitution.md**: Biết các ràng buộc và principles của project
3. **Research Phase 0**: Nếu có `[CẦN LÀM RÕ]` trong Technical Context → tạo research.md trước
4. **Chọn tech stack hợp lý**: Dựa trên:
   - Requirements từ spec.md
   - Constitution constraints
   - Industry best practices
   - Team expertise (nếu biết)
5. **Justify decisions**: Mọi lựa chọn kỹ thuật phải có lý do rõ ràng trong research.md

### Technical Context - Hướng dẫn điền

**Language/Version**:
- Chọn version cụ thể, không để "latest"
- Ví dụ: "Python 3.11" không phải "Python 3.x"

**Primary Dependencies**:
- Liệt kê frameworks/libraries chính (3-5 items)
- Ví dụ: "FastAPI 0.104, SQLAlchemy 2.0, Pydantic 2.5"

**Storage**:
- Ghi rõ loại database và version nếu cần persist data
- Nếu không cần database: "N/A - stateless"
- Ví dụ: "PostgreSQL 15 với TimescaleDB extension"

**Testing**:
- Ghi rõ test framework cho từng loại test
- Ví dụ: "pytest (unit/integration), Playwright (E2E)"

**Target Platform**:
- Môi trường chạy chính
- Ví dụ: "Docker containers trên Linux (Debian 12)"

**Project Type**:
- `single`: Một codebase duy nhất
- `web`: Backend + Frontend riêng biệt
- `mobile`: Mobile app + API backend

**Performance Goals**:
- Metrics cụ thể có thể đo được
- Ví dụ: "API response < 200ms p95, throughput 1000 req/s"

**Constraints**:
- Giới hạn kỹ thuật cụ thể
- Ví dụ: "Memory < 512MB, CPU < 2 cores, phải chạy offline"

**Scale/Scope**:
- Quy mô dự kiến
- Ví dụ: "10K active users, 1M records, 50 API endpoints"

### Constitution Check - Cách thực hiện

1. Đọc `.specify/memory/constitution.md`
2. Với mỗi principle, tự hỏi: "Plan này có vi phạm không?"
3. Nếu vi phạm:
   - Option A: Điều chỉnh plan để tuân thủ
   - Option B: Justify vi phạm trong bảng Complexity Tracking
4. Không justified được → ERROR, không tiếp tục

**Ví dụ Constitution Check**:

```markdown
## Constitution Check

### ✅ Principle I: Library-First
- Tuân thủ: Feature được thiết kế như standalone library trong src/features/
- Export qua CLI command: `myapp feature-name --action`

### ⚠️ Principle III: Test-First
- Vi phạm: Team request bỏ qua TDD cho prototype nhanh
- Justification: Xem Complexity Tracking table

### ✅ Principle V: Simplicity
- Tuân thủ: Chỉ dùng FastAPI, không thêm framework phức tạp
```

### Project Structure - Cách chọn

**Option 1 - Single Project**: Chọn khi:
- Một application đơn giản
- CLI tool, library, hoặc service đơn
- Không cần tách frontend/backend

**Option 2 - Web Application**: Chọn khi:
- Có frontend (React, Vue, etc.) + backend riêng
- Frontend và backend deploy riêng biệt
- Team frontend/backend làm việc độc lập

**Option 3 - Mobile + API**: Chọn khi:
- Mobile app (iOS/Android) + API backend
- Có thể có nhiều mobile platforms
- API có thể serve cho web sau này

**SAU KHI CHỌN**:
- XÓA 2 options không dùng
- Điền đầy đủ cấu trúc thư mục thật
- Giải thích lý do chọn trong "Quyết định Cấu trúc"

### Validation Checklist

Trước khi hoàn thành plan, tự hỏi:

- ✅ Tất cả items trong Technical Context đã điền đầy đủ?
- ✅ Constitution Check đã thực hiện và pass (hoặc justified)?
- ✅ Đã chọn rõ ràng 1 trong 3 Project Structure options?
- ✅ Mọi quyết định kỹ thuật đều có lý do rõ ràng?
- ✅ Plan này đủ chi tiết để generate tasks.md?

Nếu có câu trả lời "KHÔNG" → Bổ sung thông tin trước khi output.
