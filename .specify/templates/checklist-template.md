# Checklist [LOẠI]: [TÊN TÍNH NĂNG]

**Mục đích**: [Mô tả ngắn gọn checklist này cover những gì]
**Ngày tạo**: [DATE]
**Feature**: [Link tới spec.md hoặc tài liệu liên quan]

**Lưu ý**: Checklist này được generate bởi lệnh `/speckit.checklist` dựa trên ngữ cảnh tính năng và yêu cầu.

<!--
  ============================================================================
  QUAN TRỌNG: Các checklist items bên dưới là MẪU chỉ để minh họa.

  Lệnh /speckit.checklist PHẢI thay thế chúng bằng items thực tế dựa trên:
  - Yêu cầu checklist cụ thể của user
  - Yêu cầu tính năng từ spec.md
  - Ngữ cảnh kỹ thuật từ plan.md
  - Chi tiết implementation từ tasks.md

  KHÔNG giữ lại các sample items này trong file checklist được generate.
  ============================================================================
-->

## [Danh mục 1]

- [ ] CHK001 Checklist item đầu tiên với action rõ ràng
- [ ] CHK002 Checklist item thứ hai
- [ ] CHK003 Checklist item thứ ba

## [Danh mục 2]

- [ ] CHK004 Item của danh mục khác
- [ ] CHK005 Item với tiêu chí cụ thể
- [ ] CHK006 Item cuối cùng trong danh mục này

## Ghi chú

- Check off items khi hoàn thành: `[x]`
- Thêm comments hoặc findings inline
- Link tới tài nguyên hoặc documentation liên quan
- Items được đánh số tuần tự để dễ tham chiếu

---

## Hướng dẫn cho AI Generation (XÓA section này sau khi generate checklist thực tế)

### Loại Checklist Phổ biến

**Spec Quality Checklist**:
- Không có implementation details
- Requirements testable và rõ ràng
- Success criteria đo lường được
- User stories độc lập

**UX/UI Checklist**:
- Responsive design
- Accessibility (WCAG compliance)
- Error states và loading states
- User feedback và confirmations

**Security Checklist**:
- Input validation
- Authentication và authorization
- Data encryption
- Rate limiting
- OWASP top 10 coverage

**Performance Checklist**:
- Load time targets
- Memory usage
- Database query optimization
- Caching strategy

**Testing Checklist**:
- Unit tests coverage
- Integration tests
- E2E tests
- Edge cases covered

**Deployment Checklist**:
- Environment variables configured
- Database migrations ready
- Rollback plan
- Monitoring và alerts

### Format Checklist Items

**EVERY item MUST follow**:

```
- [ ] CHK### Mô tả action cụ thể với tiêu chí rõ ràng
```

**Components**:
- `- [ ]`: Markdown checkbox (bắt buộc)
- `CHK###`: Checklist ID tuần tự (CHK001, CHK002...)
- Mô tả: Action hoặc điều kiện rõ ràng để verify

**Ví dụ ĐÚNG**:
- `- [ ] CHK001 Verify tất cả API endpoints có rate limiting`
- `- [ ] CHK002 Confirm responsive design hoạt động trên mobile (<768px)`
- `- [ ] CHK003 Test error handling với invalid input data`

**Ví dụ SAI**:
- `CHK001 Check API` (thiếu checkbox, mơ hồ)
- `- [ ] Test mobile` (thiếu ID, không cụ thể)
- `- [ ] CHK001 Make it good` (quá mơ hồ, không actionable)

### Tổ chức Checklist

1. **Nhóm theo Categories Logic**: Nhóm items liên quan vào các sections
2. **Thứ tự ưu tiên**: Critical items trước, nice-to-have sau
3. **Độc lập**: Mỗi item phải có thể check riêng biệt
4. **Actionable**: Mỗi item phải có action hoặc tiêu chí rõ ràng
5. **Measurable**: Tránh subjective items ("make it fast" → "response time < 200ms")

### Validation Trước khi Output

- ✅ Tất cả items follow format đúng?
- ✅ Mỗi item actionable và có tiêu chí rõ ràng?
- ✅ Items được nhóm logic vào categories?
- ✅ Không có duplicate items?
- ✅ Checklist match với loại được yêu cầu (UX, Security, etc.)?

Nếu có "KHÔNG" → Sửa trước khi output.
