# ADR-001 Sinh mã Ticket

**Trạng thái:** Đề nghị cho review kỹ thuật.

## Bối cảnh

Cần mã dễ tra cứu và duy nhất khi nhiều request tạo cùng lúc; mã giữ sau chuyển phòng ban.

## Quyết định

Dùng khóa ticket_id do database cấp; tạo code US-YYYY-ID với YYYY theo created_at giờ Việt Nam, ID đệm tối thiểu 6 chữ số nhưng không cắt nếu lớn hơn. Đặt unique trên code. Cấp ID và ghi code trong transaction trước commit, không hiển thị record chưa hoàn tất.

## Hệ quả và kiểm chứng

Không dùng COUNT+1, timestamp đơn lẻ hoặc mã mang phòng ban hiện tại. Hai tạo đồng thời không trùng; retry dùng operation_id trả cùng code. Khoảng trống số sau rollback được chấp nhận, không tái sử dụng mã.

## Yêu cầu liên quan

FR-STU-02; BR-03, BR-08, BR-10; AC-STU-02-01, AC-STU-02-05.

[Về danh mục PRD](../../README.md)
