# Đặc tả Tính năng: [TÊN TÍNH NĂNG]

**Feature Branch**: `[###-feature-name]`
**Ngày tạo**: [DATE]
**Trạng thái**: Nháp
**Input từ user**: "$ARGUMENTS"

## Kịch bản Người dùng & Kiểm thử *(bắt buộc)*

<!--
  QUAN TRỌNG: Các user stories phải được ƯU TIÊN theo thứ tự journey quan trọng nhất.
  Mỗi user story/journey phải có khả năng KIỂM THỬ ĐỘC LẬP - nghĩa là nếu chỉ implement MỘT story,
  vẫn có một MVP (Minimum Viable Product) khả dụng và mang lại giá trị.

  Gán mức ưu tiên (P1, P2, P3, v.v.) cho mỗi story, trong đó P1 là quan trọng nhất.
  Coi mỗi story như một phần chức năng độc lập có thể:
  - Phát triển độc lập
  - Kiểm thử độc lập
  - Deploy độc lập
  - Demo cho users độc lập
-->

### User Story 1 - [Tiêu đề ngắn gọn] (Ưu tiên: P1)

[Mô tả journey của người dùng bằng ngôn ngữ đơn giản, dễ hiểu]

**Tại sao ưu tiên này**: [Giải thích giá trị và lý do có mức ưu tiên này]

**Kiểm thử độc lập**: [Mô tả cách có thể kiểm thử độc lập story này - ví dụ: "Có thể kiểm thử đầy đủ bằng cách [hành động cụ thể] và mang lại [giá trị cụ thể]"]

**Kịch bản chấp nhận**:

1. **Cho trước** [trạng thái ban đầu], **Khi** [hành động], **Thì** [kết quả mong đợi]
2. **Cho trước** [trạng thái ban đầu], **Khi** [hành động], **Thì** [kết quả mong đợi]

---

### User Story 2 - [Tiêu đề ngắn gọn] (Ưu tiên: P2)

[Mô tả journey của người dùng bằng ngôn ngữ đơn giản, dễ hiểu]

**Tại sao ưu tiên này**: [Giải thích giá trị và lý do có mức ưu tiên này]

**Kiểm thử độc lập**: [Mô tả cách có thể kiểm thử độc lập story này]

**Kịch bản chấp nhận**:

1. **Cho trước** [trạng thái ban đầu], **Khi** [hành động], **Thì** [kết quả mong đợi]

---

### User Story 3 - [Tiêu đề ngắn gọn] (Ưu tiên: P3)

[Mô tả journey của người dùng bằng ngôn ngữ đơn giản, dễ hiểu]

**Tại sao ưu tiên này**: [Giải thích giá trị và lý do có mức ưu tiên này]

**Kiểm thử độc lập**: [Mô tả cách có thể kiểm thử độc lập story này]

**Kịch bản chấp nhận**:

1. **Cho trước** [trạng thái ban đầu], **Khi** [hành động], **Thì** [kết quả mong đợi]

---

[Thêm user stories khác nếu cần, mỗi story đều phải có mức ưu tiên]

### Trường hợp Biên

<!--
  HÀNH ĐỘNG CẦN THIẾT: Nội dung trong section này là placeholder.
  Điền các trường hợp biên phù hợp.
-->

- Điều gì xảy ra khi [điều kiện biên]?
- Hệ thống xử lý thế nào khi [tình huống lỗi]?

## Yêu cầu *(bắt buộc)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Nội dung trong section này là placeholder.
  Điền các yêu cầu chức năng phù hợp.
-->

### Yêu cầu Chức năng

- **FR-001**: Hệ thống PHẢI [khả năng cụ thể, ví dụ: "cho phép người dùng tạo tài khoản"]
- **FR-002**: Hệ thống PHẢI [khả năng cụ thể, ví dụ: "xác thực địa chỉ email"]
- **FR-003**: Người dùng PHẢI có khả năng [tương tác quan trọng, ví dụ: "đặt lại mật khẩu"]
- **FR-004**: Hệ thống PHẢI [yêu cầu về dữ liệu, ví dụ: "lưu trữ preferences của người dùng"]
- **FR-005**: Hệ thống PHẢI [hành vi, ví dụ: "ghi log tất cả các sự kiện bảo mật"]

*Ví dụ đánh dấu yêu cầu chưa rõ ràng:*

- **FR-006**: Hệ thống PHẢI xác thực người dùng qua [CẦN LÀM RÕ: phương thức xác thực chưa được chỉ định - email/password, SSO, OAuth?]
- **FR-007**: Hệ thống PHẢI lưu trữ dữ liệu người dùng trong [CẦN LÀM RÕ: thời gian lưu trữ chưa được chỉ định]

### Entities Chính *(chỉ bao gồm nếu tính năng liên quan đến dữ liệu)*

- **[Entity 1]**: [Đại diện cho gì, các thuộc tính chính không đề cập đến implementation]
- **[Entity 2]**: [Đại diện cho gì, mối quan hệ với các entities khác]

## Tiêu chí Thành công *(bắt buộc)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Định nghĩa các tiêu chí thành công có thể đo lường được.
  Các tiêu chí này PHẢI độc lập với công nghệ và có thể đo lường được.
-->

### Kết quả Đo lường được

- **SC-001**: [Metric đo lường được, ví dụ: "Người dùng có thể hoàn thành việc tạo tài khoản trong vòng dưới 2 phút"]
- **SC-002**: [Metric đo lường được, ví dụ: "Hệ thống xử lý 1000 người dùng đồng thời mà không bị giảm hiệu suất"]
- **SC-003**: [Metric về sự hài lòng của user, ví dụ: "90% người dùng hoàn thành tác vụ chính thành công ngay lần đầu tiên"]
- **SC-004**: [Metric về business, ví dụ: "Giảm 50% số ticket hỗ trợ liên quan đến [X]"]

## Giả định *(optional - chỉ khi có giả định quan trọng)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Liệt kê các giả định được đưa ra khi tạo spec này.
  Ví dụ: "Giả định người dùng đã có tài khoản email", "Giả định hệ thống chạy trên môi trường cloud"
-->

- [Giả định 1]
- [Giả định 2]

## Phụ thuộc *(optional - chỉ khi có dependencies bên ngoài)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Liệt kê các dependencies với hệ thống/service khác.
  Ví dụ: "Phụ thuộc vào API OAuth của Google", "Cần database PostgreSQL"
-->

- [Dependency 1]
- [Dependency 2]

## Ràng buộc *(optional - chỉ khi có constraints cụ thể)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Liệt kê các ràng buộc kỹ thuật, business, hoặc pháp lý.
  Ví dụ: "Phải tuân thủ GDPR", "Phải hoạt động trên mobile devices", "Budget giới hạn $X"
-->

- [Constraint 1]
- [Constraint 2]

## Out of Scope *(optional - chỉ khi cần làm rõ ranh giới)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Làm rõ những gì KHÔNG nằm trong phạm vi tính năng này.
  Giúp tránh scope creep và expectations sai lệch.
-->

- [Tính năng X không thuộc phạm vi của spec này]
- [Yêu cầu Y sẽ được xử lý trong tính năng khác]

---

## Hướng dẫn Sử dụng Template (XÓA section này sau khi hoàn thành spec)

### Nguyên tắc Chung

- Tập trung vào **CÁI GÌ** (WHAT) người dùng cần và **TẠI SAO** (WHY).
- Tránh **LÀM THẾ NÀO** (HOW) để implement (không đề cập tech stack, APIs, cấu trúc code).
- Viết cho business stakeholders, không phải developers.
- KHÔNG tạo bất kỳ checklist nào nhúng trong spec. Checklist sẽ là command riêng biệt.

### Yêu cầu về Sections

- **Sections bắt buộc**: Phải hoàn thành cho mọi tính năng
- **Sections optional**: Chỉ bao gồm khi có liên quan đến tính năng
- Khi một section không áp dụng, xóa hoàn toàn (đừng để là "N/A")

### Cho AI Generation

Khi tạo spec này từ user prompt:

1. **Đưa ra suy đoán có căn cứ**: Sử dụng context, chuẩn ngành, và patterns phổ biến để lấp khoảng trống
2. **Ghi lại assumptions**: Ghi các giá trị mặc định hợp lý trong section Assumptions
3. **Giới hạn clarifications**: Tối đa 3 marker `[CẦN LÀM RÕ:]` - chỉ dùng cho quyết định quan trọng mà:
   - Ảnh hưởng đáng kể đến phạm vi hoặc trải nghiệm người dùng
   - Có nhiều cách hiểu hợp lý với hệ quả khác nhau
   - Thiếu bất kỳ giá trị mặc định hợp lý nào
4. **Ưu tiên clarifications**: scope > security/privacy > user experience > technical details
5. **Tư duy như tester**: Mọi requirement mơ hồ phải fail checklist item "testable và rõ ràng"
6. **Các vấn đề thường cần clarification** (chỉ khi không có giá trị mặc định hợp lý):
   - Phạm vi tính năng và ranh giới (include/exclude use cases cụ thể)
   - Loại người dùng và quyền hạn (nếu có nhiều cách hiểu conflict)
   - Yêu cầu security/compliance (khi có ý nghĩa pháp lý/tài chính)

**Ví dụ về giá trị mặc định hợp lý** (không cần hỏi):

- Data retention: Thực hành chuẩn ngành cho domain đó
- Performance targets: Kỳ vọng chuẩn cho web/mobile app trừ khi có chỉ định khác
- Error handling: Messages thân thiện với user và fallbacks phù hợp
- Authentication method: Session-based hoặc OAuth2 chuẩn cho web apps
- Integration patterns: RESTful APIs trừ khi có chỉ định khác

### Hướng dẫn Success Criteria

Success criteria phải:

1. **Đo lường được**: Bao gồm metrics cụ thể (time, percentage, count, rate)
2. **Độc lập công nghệ**: Không đề cập frameworks, languages, databases, hoặc tools
3. **Tập trung vào user**: Mô tả kết quả từ góc nhìn user/business, không phải system internals
4. **Có thể xác minh**: Có thể test/validate mà không cần biết implementation details

**Ví dụ TỐT**:

- "Người dùng có thể hoàn thành checkout trong vòng dưới 3 phút"
- "Hệ thống hỗ trợ 10,000 người dùng đồng thời"
- "95% tìm kiếm trả về kết quả trong vòng dưới 1 giây"
- "Tỷ lệ hoàn thành task tăng 40%"

**Ví dụ TỆ** (tập trung vào implementation):

- "API response time dưới 200ms" (quá kỹ thuật, dùng "Người dùng thấy kết quả ngay lập tức")
- "Database có thể xử lý 1000 TPS" (implementation detail, dùng metric hướng user)
- "React components render hiệu quả" (framework-specific)
- "Redis cache hit rate trên 80%" (technology-specific)

### Validation Checklist Tự kiểm tra

Trước khi hoàn thành spec, tự hỏi:

- ✅ Spec này có người không biết lập trình đọc được không?
- ✅ Tất cả requirements đều testable và rõ ràng?
- ✅ Success criteria có thể đo lường được và không có tech details?
- ✅ User stories độc lập và có thể test riêng biệt?
- ✅ Không có đề cập đến languages, frameworks, APIs cụ thể?

Nếu có bất kỳ câu trả lời "KHÔNG" → Sửa lại trước khi gửi spec.
