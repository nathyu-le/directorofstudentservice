# Mô hình lưu trữ và ràng buộc

| Bảng đề nghị | Trường cốt lõi | Khóa và ràng buộc |
| --- | --- | --- |
| users | id, email, password_hash, role, department_id, active | email unique; NV có department_id. |
| departments | id, name, active | Khóa chính; không xóa khi đã tham chiếu. |
| manager_departments | manager_id, department_id | Cặp unique; chỉ QL được quản lý. |
| categories | id, department_id, name, sla_hours, active | Loại thuộc một phòng ban; sla_hours 1 đến 168. |
| tickets | Các trường ở ticket-model; active_information_request_id | code unique; FK chủ/loại/PB/NV; version ≥1; cặp loại/PB hợp lệ. |
| progress_updates | id, ticket_id, actor_id, content, created_at | FK Ticket; nội dung và quyền theo FR-OPS-03. |
| information_requests | id, ticket_id, requested_by, content, status, requested_at | OPEN hoặc ANSWERED; Ticket giữ khóa active request để bảo đảm chỉ một OPEN. |
| information_answers | id, information_request_id, student_id, content, answered_at | information_request_id unique; chủ SV đúng Ticket. |
| audit_events | id, ticket_id, type, actor_id, actor_name_snapshot, occurred_at, before, after | Chỉ insert nghiệp vụ; có actor system khi quét hạn. |
| feedback | id, ticket_id, student_id, rating, comment, submitted_at | ticket_id unique; rating nguyên 1 đến 5; chỉ sau CLOSED. |
| idempotency_results | actor_id, action, scope, operation_id, payload_hash, result_ref | Khóa kết hợp unique; lưu cùng transaction; result tối thiểu. |
| domain_events | event_id, ticket_id, event_type, payload_minimal, created_at | event_id unique; recipient chụp tại commit. |
| event_recipients | event_id, recipient_id, processed_at, retry_count, last_error_code | Cặp unique; tiến độ riêng từng recipient. |
| notifications | id, event_id, recipient_id, ticket_id, message, created_at, read_at | event_id/recipient_id unique; read_at lần đầu. |
| overdue_events | ticket_id, due_at, event_id | ticket_id/due_at unique; không dùng một boolean để mất lịch sử cảnh báo. |


Các bảng sử dụng engine hỗ trợ transaction. Ràng buộc liên bảng khó biểu diễn bằng một khóa phải được domain service kiểm trong giao dịch và có ca test. Không chỉ dựa vào validate ở FE. Gợi ý chỉ mục: tickets(student_id, created_at, id), tickets(department_id, status, assignee_id), tickets(assignee_id, status), tickets(status, due_at), audit_events(ticket_id, occurred_at, id), notifications(recipient_id, read_at, created_at, id). BE kiểm execution plan ở dữ liệu 10000 Ticket trước chốt chỉ mục.

active_information_request_id được đặt khi tạo câu hỏi và xóa khi trả lời, cùng giao dịch Ticket. Giữ câu hỏi/answer cũ để đọc hội thoại. Không dùng khóa unique(ticket_id) trên toàn information_requests vì một Ticket được có nhiều vòng bổ sung. Các báo cáo join feedback tối đa một bản, còn events/hội thoại phải aggregate hoặc tránh join làm nhân số Ticket.

[Về danh mục PRD](../README.md)
