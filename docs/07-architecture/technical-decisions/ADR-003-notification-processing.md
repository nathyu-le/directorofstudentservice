# ADR-003 Xử lý thông báo sau giao dịch

**Trạng thái:** Đề nghị cho review kỹ thuật.

## Bối cảnh

Thông báo cần không mất khi process dừng nhưng không khiến Ticket đã lưu bị rollback do một recipient lỗi.

## Quyết định

Lưu domain event và recipient snapshot trong transaction nghiệp vụ; xử lý từng recipient sau commit. Unique event_id/recipient_id trên notification; processed_at và last_error_code theo recipient. Job retry các phần chưa hoàn thành; polling tạo thông báo trong 60 giây khi hoạt động.

## Hệ quả và kiểm chứng

Không gửi email/SMS. Job có thể chạy nhiều lần mà không trùng; một recipient lỗi không mất phần khác. Payload tối thiểu. OVERDUE_DETECTED chỉ duy nhất theo ticket_id/due_at và chỉ khi trạng thái còn mở lúc ghi.

## Yêu cầu liên quan

FR-NOT-01, FR-SLA-03; BR-08, BR-13; NFR-REL-02, NFR-OBS-02.

[Về danh mục PRD](../../README.md)
