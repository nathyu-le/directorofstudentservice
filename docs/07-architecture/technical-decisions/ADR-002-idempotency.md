# ADR-002 Chống xử lý lặp thao tác ghi

**Trạng thái:** Đề nghị cho review kỹ thuật.

## Bối cảnh

Double click và retry sau mất response có thể lặp tạo Ticket, câu hỏi, kết quả hoặc feedback.

## Quyết định

Dùng operation_id UUID theo actor/action/scope, payload_hash và result tối thiểu; khóa unique trong database. Đọc kết quả đã commit trước kiểm version của retry. Nếu request cùng khóa còn xử lý, đợi kết quả transaction hoặc trả xung đột tạm có thể retry; không chạy nghiệp vụ lần hai.

## Hệ quả và kiểm chứng

Cùng mã khác payload bị chặn. Retry không tăng version, event hoặc notification. Hai mã khác vẫn phải tuân thủ unique nghiệp vụ và điều kiện trạng thái. Replay sau mất quyền chỉ trả xác nhận tối thiểu, không nội dung Ticket.

## Yêu cầu liên quan

BR-08, BR-09, BR-10; FR-STU-02, FR-COM-02, FR-FDB-02; NFR-REL-01.

[Về danh mục PRD](../../README.md)
