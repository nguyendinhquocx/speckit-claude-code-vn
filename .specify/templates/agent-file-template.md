# Hướng dẫn Phát triển [TÊN PROJECT]

Tự động generate từ tất cả feature plans. Cập nhật lần cuối: [DATE]

## Công nghệ Đang sử dụng

[TRÍCH XUẤT TỪ TẤT CẢ CÁC FILE PLAN.MD]

## Cấu trúc Project

```text
[CẤU TRÚC THỰC TẾ TỪ PLANS]
```

## Commands

[CHỈ CÁC COMMANDS CHO CÔNG NGHỆ ĐANG SỬ DỤNG]

## Code Style

[THEO NGÔN NGỮ CỤ THỂ, CHỈ CHO NGÔN NGỮ ĐANG DÙNG]

## Thay đổi Gần đây

[3 FEATURES GẦN NHẤT VÀ NHỮNG GÌ CHÚNG ĐÃ THÊM VÀO]

<!-- MANUAL ADDITIONS START -->
<!-- MANUAL ADDITIONS END -->

---

## Hướng dẫn cho AI Update (XÓA section này khi tạo file agent thực tế)

### Mục đích File

File này serve như **context file cho AI agent** để:
- Hiểu công nghệ đang được sử dụng trong project
- Biết cấu trúc thư mục và conventions
- Có commands sẵn để run tasks
- Maintain consistency across features

### Khi nào Update

File này được update bởi lệnh:
- `/speckit.plan` - sau Phase 1 khi có technical decisions
- Script `.specify/scripts/powershell/update-agent-context.ps1`

### Sections Bắt buộc

**1. Công nghệ Đang sử dụng**:
- Extract từ tất cả `plan.md` files trong `specs/*/plan.md`
- Chỉ list công nghệ đang THẬT SỰ được dùng trong codebase
- Format: Category → Technologies
- Ví dụ:
  ```markdown
  ## Công nghệ Đang sử dụng

  **Languages**: Python 3.11, TypeScript 5.2
  **Backend**: FastAPI 0.104, SQLAlchemy 2.0
  **Frontend**: React 18, Tailwind CSS 3.3
  **Database**: PostgreSQL 15
  **Testing**: pytest, Playwright
  **DevOps**: Docker, GitHub Actions
  ```

**2. Cấu trúc Project**:
- Hiển thị cấu trúc thư mục THỰC TẾ
- Bao gồm comments giải thích mỗi thư mục
- Ví dụ:
  ```markdown
  ## Cấu trúc Project

  \```text
  backend/
  ├── src/
  │   ├── models/        # Database models (SQLAlchemy)
  │   ├── services/      # Business logic
  │   ├── api/           # FastAPI endpoints
  │   └── lib/           # Shared utilities
  ├── tests/
  │   ├── contract/      # API contract tests
  │   ├── integration/   # Integration tests
  │   └── unit/          # Unit tests
  └── migrations/        # Database migrations (Alembic)

  frontend/
  ├── src/
  │   ├── components/    # React components
  │   ├── pages/         # Page components
  │   ├── hooks/         # Custom React hooks
  │   └── services/      # API client code
  └── public/            # Static assets
  \```
  ```

**3. Commands**:
- Common commands cho technologies được sử dụng
- Grouped by task type
- Ví dụ:
  ```markdown
  ## Commands

  **Development**:
  \```bash
  # Backend
  cd backend && uvicorn src.main:app --reload

  # Frontend
  cd frontend && npm run dev
  \```

  **Testing**:
  \```bash
  # Run all tests
  pytest backend/tests

  # Run specific test
  pytest backend/tests/unit/test_user_service.py
  \```

  **Database**:
  \```bash
  # Create migration
  alembic revision --autogenerate -m "description"

  # Run migrations
  alembic upgrade head
  \```
  ```

**4. Code Style**:
- Conventions cho ngôn ngữ đang dùng
- Naming conventions
- Import organization
- Ví dụ (Python):
  ```markdown
  ## Code Style

  **Python (backend)**:
  - Follow PEP 8
  - Use type hints cho tất cả functions
  - Docstrings: Google style
  - Import order: stdlib → third-party → local
  - Max line length: 100 characters

  **TypeScript (frontend)**:
  - Follow Airbnb style guide
  - Components: PascalCase
  - Functions/variables: camelCase
  - Use functional components với hooks
  ```

**5. Thay đổi Gần đây**:
- 3 features gần nhất đã implement
- Công nghệ mới được thêm vào
- Ví dụ:
  ```markdown
  ## Thay đổi Gần đây

  **Feature 003-user-auth** (2025-01-15):
  - Thêm JWT authentication
  - Thêm Redis cho session management
  - New endpoints: /api/auth/login, /api/auth/logout

  **Feature 002-product-catalog** (2025-01-10):
  - Thêm Elasticsearch cho full-text search
  - New models: Product, Category
  - Pagination với cursor-based approach

  **Feature 001-database-setup** (2025-01-05):
  - Initial PostgreSQL setup
  - Alembic migrations configured
  - Base models: User, Timestamp mixin
  ```

### Manual Additions Section

```markdown
<!-- MANUAL ADDITIONS START -->
<!-- Team có thể thêm custom notes, decisions, gotchas ở đây -->
<!-- Content này sẽ KHÔNG bị overwrite bởi automated updates -->
<!-- MANUAL ADDITIONS END -->
```

### Update Logic

**Script update process**:
1. Scan tất cả `specs/*/plan.md` files
2. Extract technologies từ Technical Context sections
3. Deduplicate và organize by category
4. Preserve content giữa `<!-- MANUAL ADDITIONS START -->` và `<!-- MANUAL ADDITIONS END -->`
5. Update file với new info

**QUAN TRỌNG**: KHÔNG BAO GIỜ xóa nội dung giữa MANUAL ADDITIONS markers.
