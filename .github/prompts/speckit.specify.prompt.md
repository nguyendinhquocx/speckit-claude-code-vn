---
description: Tạo hoặc cập nhật đặc tả tính năng từ mô tả tính năng bằng ngôn ngữ tự nhiên.
---

## Input từ User

```text
$ARGUMENTS
```

Mày **PHẢI** xem xét input từ user trước khi tiếp tục (nếu không rỗng).

## Outline

Văn bản mà user gõ sau `/speckit.specify` trong triggering message **chính là** mô tả tính năng. Giả định mày luôn có nó available trong conversation này ngay cả khi `$ARGUMENTS` xuất hiện literally bên dưới. Đừng yêu cầu user lặp lại trừ khi họ cung cấp command rỗng.

Với mô tả tính năng đó, làm như sau:

1. **Generate một short name ngắn gọn** (2-4 từ) cho branch:
   - Phân tích mô tả tính năng và trích xuất các keywords ý nghĩa nhất
   - Tạo short name 2-4 từ capture được essence của tính năng
   - Dùng action-noun format khi có thể (ví dụ: "add-user-auth", "fix-payment-bug")
   - Giữ nguyên technical terms và acronyms (OAuth2, API, JWT, etc.)
   - Giữ ngắn gọn nhưng đủ descriptive để hiểu tính năng ngay lập tức
   - Ví dụ:
     - "Tôi muốn thêm xác thực người dùng" → "user-auth"
     - "Triển khai OAuth2 integration cho API" → "oauth2-api-integration"
     - "Tạo dashboard cho analytics" → "analytics-dashboard"
     - "Sửa bug timeout xử lý thanh toán" → "fix-payment-timeout"

2. **Kiểm tra branches hiện có trước khi tạo mới**:

   a. Đầu tiên, fetch tất cả remote branches để đảm bảo có thông tin mới nhất:
      ```bash
      git fetch --all --prune
      ```

   b. Tìm feature number cao nhất across all sources cho short-name:
      - Remote branches: `git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - Local branches: `git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - Specs directories: Kiểm tra directories matching `specs/[0-9]+-<short-name>`

   c. Xác định số tiếp theo available:
      - Extract tất cả numbers từ cả ba sources
      - Tìm số cao nhất N
      - Dùng N+1 cho branch number mới

   d. Chạy script `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS"` với số đã tính và short-name:
      - Pass `--number N+1` và `--short-name "your-short-name"` cùng với mô tả tính năng
      - Bash example: `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" --json --number 5 --short-name "user-auth" "Add user authentication"`
      - PowerShell example: `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" -Json -Number 5 -ShortName "user-auth" "Add user authentication"`

   **QUAN TRỌNG**:
   - Kiểm tra cả ba sources (remote branches, local branches, specs directories) để tìm số cao nhất
   - Chỉ match branches/directories với exact short-name pattern
   - Nếu không tìm thấy branches/directories nào với short-name này, bắt đầu với số 1
   - Mày chỉ được chạy script này MỘT LẦN DUY NHẤT mỗi feature
   - JSON được cung cấp trong terminal output - luôn tham chiếu nó để lấy nội dung thực tế mày đang tìm
   - JSON output sẽ chứa BRANCH_NAME và SPEC_FILE paths
   - Với single quotes trong args như "I'm Groot", dùng escape syntax: ví dụ 'I'\''m Groot' (hoặc double-quote nếu có thể: "I'm Groot")

3. Load `.specify/templates/spec-template.md` để hiểu các sections bắt buộc.

4. Follow execution flow này:

    1. Parse mô tả user từ Input
       Nếu rỗng: ERROR "Không có mô tả tính năng được cung cấp"
    2. Extract các khái niệm chính từ mô tả
       Identify: actors, actions, data, constraints
    3. Với các khía cạnh chưa rõ ràng:
       - Đưa ra suy đoán có căn cứ dựa trên context và industry standards
       - Chỉ đánh dấu với [CẦN LÀM RÕ: câu hỏi cụ thể] nếu:
         - Lựa chọn ảnh hưởng đáng kể đến phạm vi tính năng hoặc trải nghiệm người dùng
         - Có nhiều cách hiểu hợp lý với hệ quả khác nhau
         - Không có giá trị mặc định hợp lý
       - **GIỚI HẠN: Tối đa 3 markers [CẦN LÀM RÕ] tổng cộng**
       - Ưu tiên clarifications theo impact: scope > security/privacy > user experience > technical details
    4. Điền section Kịch bản Người dùng & Kiểm thử
       Nếu không có user flow rõ ràng: ERROR "Không thể xác định kịch bản người dùng"
    5. Generate Yêu cầu Chức năng
       Mỗi yêu cầu phải testable
       Dùng giá trị mặc định hợp lý cho chi tiết chưa chỉ định (document assumptions trong section Giả định)
    6. Define Tiêu chí Thành công
       Tạo kết quả measurable, độc lập công nghệ
       Bao gồm cả quantitative metrics (time, performance, volume) và qualitative measures (user satisfaction, task completion)
       Mỗi tiêu chí phải verifiable mà không cần implementation details
    7. Identify Key Entities (nếu liên quan đến data)
    8. Return: SUCCESS (spec sẵn sàng cho planning)

5. Viết specification vào SPEC_FILE sử dụng cấu trúc template, thay thế placeholders bằng chi tiết cụ thể derived từ mô tả tính năng (arguments) trong khi preserve thứ tự sections và headings.

   **NGÔN NGỮ OUTPUT BẮT BUỘC**:
   - **TẤT CẢ nội dung mô tả PHẢI bằng TIẾNG VIỆT**
   - Giữ nguyên: technical terms (API, REST, OAuth), entity names (User, Product), keywords ([CẦN LÀM RÕ], FR-001), file paths
   - Trước khi viết file, tự kiểm tra: "Spec này có người Việt không biết tiếng Anh đọc được không?"
   - Nếu phát hiện câu mô tả toàn tiếng Anh → DỪNG và viết lại bằng tiếng Việt

6. **Validation Chất lượng Specification**: Sau khi viết spec ban đầu, validate nó theo tiêu chí chất lượng:

   a. **Tạo Spec Quality Checklist**: Generate file checklist tại `FEATURE_DIR/checklists/requirements.md` sử dụng cấu trúc checklist template với các validation items này:

      ```markdown
      # Checklist Chất lượng Specification: [TÊN TÍNH NĂNG]

      **Mục đích**: Validate tính đầy đủ và chất lượng specification trước khi tiến hành planning
      **Ngày tạo**: [DATE]
      **Feature**: [Link tới spec.md]

      ## Chất lượng Nội dung

      - [ ] Không có implementation details (ngôn ngữ, frameworks, APIs)
      - [ ] Tập trung vào giá trị cho user và business needs
      - [ ] Viết cho non-technical stakeholders
      - [ ] Tất cả mandatory sections đã hoàn thành

      ## Tính Đầy đủ Yêu cầu

      - [ ] Không còn markers [CẦN LÀM RÕ]
      - [ ] Requirements testable và rõ ràng
      - [ ] Success criteria đo lường được
      - [ ] Success criteria độc lập công nghệ (không có implementation details)
      - [ ] Tất cả acceptance scenarios đã được định nghĩa
      - [ ] Edge cases đã được identify
      - [ ] Scope được giới hạn rõ ràng
      - [ ] Dependencies và assumptions đã được identify

      ## Sẵn sàng Feature

      - [ ] Tất cả functional requirements có acceptance criteria rõ ràng
      - [ ] User scenarios cover primary flows
      - [ ] Feature đáp ứng measurable outcomes định nghĩa trong Success Criteria
      - [ ] Không có implementation details leak vào specification

      ## Ghi chú

      - Items đánh dấu incomplete cần spec updates trước `/speckit.clarify` hoặc `/speckit.plan`
      ```

   b. **Chạy Validation Check**: Review spec theo từng checklist item:
      - Với mỗi item, xác định nó pass hay fail
      - Document các issues cụ thể tìm thấy (quote các spec sections liên quan)

   c. **Xử lý Validation Results**:

      - **Nếu tất cả items pass**: Đánh dấu checklist complete và tiến tới bước 7

      - **Nếu items fail (excluding [CẦN LÀM RÕ])**:
        1. List các failing items và issues cụ thể
        2. Update spec để address từng issue
        3. Re-run validation cho đến khi tất cả items pass (max 3 iterations)
        4. Nếu vẫn failing sau 3 iterations, document remaining issues trong checklist notes và warn user

      - **Nếu còn markers [CẦN LÀM RÕ]**:
        1. Extract tất cả markers [CẦN LÀM RÕ: ...] từ spec
        2. **LIMIT CHECK**: Nếu có hơn 3 markers, chỉ giữ 3 cái critical nhất (theo scope/security/UX impact) và đưa ra suy đoán có căn cứ cho phần còn lại
        3. Với mỗi clarification cần thiết (max 3), present options cho user theo format này:

           ```markdown
           ## Câu hỏi [N]: [Topic]

           **Ngữ cảnh**: [Quote spec section liên quan]

           **Cần biết**: [Câu hỏi cụ thể từ marker CẦN LÀM RÕ]

           **Các lựa chọn đề xuất**:

           | Option | Trả lời | Hệ quả |
           |--------|---------|--------|
           | A      | [Trả lời đề xuất thứ nhất] | [Điều này có nghĩa gì cho tính năng] |
           | B      | [Trả lời đề xuất thứ hai] | [Điều này có nghĩa gì cho tính năng] |
           | C      | [Trả lời đề xuất thứ ba] | [Điều này có nghĩa gì cho tính năng] |
           | Custom | Cung cấp trả lời riêng của bạn | [Giải thích cách cung cấp custom input] |

           **Lựa chọn của bạn**: _[Đợi user response]_
           ```

        4. **CRITICAL - Table Formatting**: Đảm bảo markdown tables được format đúng:
           - Dùng spacing nhất quán với pipes aligned
           - Mỗi cell phải có spaces xung quanh content: `| Content |` không phải `|Content|`
           - Header separator phải có ít nhất 3 dashes: `|--------|`
           - Test bảng render đúng trong markdown preview
        5. Đánh số câu hỏi tuần tự (Q1, Q2, Q3 - max 3 total)
        6. Present tất cả câu hỏi cùng nhau trước khi đợi responses
        7. Đợi user respond với lựa chọn cho tất cả câu hỏi (ví dụ: "Q1: A, Q2: Custom - [details], Q3: B")
        8. Update spec bằng cách thay thế mỗi marker [CẦN LÀM RÕ] với câu trả lời được chọn hoặc cung cấp của user
        9. Re-run validation sau khi tất cả clarifications được resolve

   d. **Update Checklist**: Sau mỗi validation iteration, update checklist file với current pass/fail status

7. Report completion với branch name, spec file path, checklist results, và readiness cho phase tiếp theo (`/speckit.clarify` hoặc `/speckit.plan`).

**LƯU Ý:** Script tạo và checkout branch mới và khởi tạo spec file trước khi viết.

## General Guidelines

## Quick Guidelines

- Tập trung vào **CÁI GÌ** (WHAT) users cần và **TẠI SAO** (WHY).
- Tránh LÀM THẾ NÀO (HOW) để implement (no tech stack, APIs, code structure).
- Viết cho business stakeholders, không phải developers.
- KHÔNG tạo bất kỳ checklists nào nhúng trong spec. Đó sẽ là command riêng biệt.

### Yêu cầu về Sections

- **Mandatory sections**: Phải hoàn thành cho mọi feature
- **Optional sections**: Chỉ bao gồm khi liên quan đến feature
- Khi một section không áp dụng, xóa hoàn toàn (đừng để là "N/A")

### Cho AI Generation

Khi tạo spec này từ user prompt:

1. **Đưa ra suy đoán có căn cứ**: Dùng context, industry standards, và common patterns để lấp khoảng trống
2. **Document assumptions**: Ghi các giá trị mặc định hợp lý trong section Giả định
3. **Giới hạn clarifications**: Tối đa 3 markers [CẦN LÀM RÕ] - chỉ dùng cho quyết định critical mà:
   - Ảnh hưởng đáng kể đến feature scope hoặc user experience
   - Có nhiều cách hiểu hợp lý với hệ quả khác nhau
   - Thiếu bất kỳ giá trị mặc định hợp lý nào
4. **Ưu tiên clarifications**: scope > security/privacy > user experience > technical details
5. **Tư duy như tester**: Mọi vague requirement phải fail checklist item "testable và rõ ràng"
6. **Các vấn đề thường cần clarification** (chỉ khi không có giá trị mặc định hợp lý):
   - Feature scope và boundaries (include/exclude use cases cụ thể)
   - User types và permissions (nếu có nhiều cách hiểu conflict)
   - Security/compliance requirements (khi có ý nghĩa pháp lý/tài chính)

**Ví dụ về giá trị mặc định hợp lý** (đừng hỏi về những thứ này):

- Data retention: Thực hành chuẩn ngành cho domain đó
- Performance targets: Kỳ vọng chuẩn web/mobile app trừ khi có chỉ định
- Error handling: Messages thân thiện với user và fallbacks phù hợp
- Authentication method: Session-based chuẩn hoặc OAuth2 cho web apps
- Integration patterns: RESTful APIs trừ khi có chỉ định khác

### Hướng dẫn Success Criteria

Success criteria phải:

1. **Đo lường được**: Bao gồm metrics cụ thể (time, percentage, count, rate)
2. **Độc lập công nghệ**: Không đề cập frameworks, languages, databases, hoặc tools
3. **Tập trung vào user**: Mô tả outcomes từ góc nhìn user/business, không phải system internals
4. **Có thể verify**: Có thể test/validate mà không cần biết implementation details

**Ví dụ tốt**:

- "Người dùng có thể hoàn thành checkout trong vòng dưới 3 phút"
- "Hệ thống hỗ trợ 10,000 người dùng đồng thời"
- "95% tìm kiếm trả về kết quả trong vòng dưới 1 giây"
- "Tỷ lệ hoàn thành task tăng 40%"

**Ví dụ tệ** (tập trung vào implementation):

- "API response time dưới 200ms" (quá kỹ thuật, dùng "Người dùng thấy kết quả ngay lập tức")
- "Database có thể xử lý 1000 TPS" (implementation detail, dùng user-facing metric)
- "React components render hiệu quả" (framework-specific)
- "Redis cache hit rate trên 80%" (technology-specific)
