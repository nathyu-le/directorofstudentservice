# Hợp đồng API đề nghị

| FR | Method | Route hoặc điểm xử lý | Đầu vào và kết quả |
| --- | --- | --- | --- |
| FR-IAM-01 | POST | /api/session | Đăng nhập; đổi mã phiên. |
| FR-IAM-02 | DELETE | /api/session | Vô hiệu phiên; CSRF. |
| FR-IAM-03 | Middleware | Mọi endpoint | Kiểm quyền tại máy chủ; không có endpoint tự cấp role. |
| FR-STU-01 | GET | /api/my/tickets | keyword, status, page; chủ từ phiên. |
| FR-STU-02 | POST | /api/tickets | category_id, title, description, operation_id. |
| FR-STU-03 | GET | /api/my/tickets/{id} | Thông tin sinh viên, thao tác hiện được phép. |
| FR-TKT-01 | GET | /api/work/tickets | view và bộ lọc theo phạm vi. |
| FR-TKT-02 | POST | /api/tickets/{id}/category | category_id, reason, version, operation_id. |
| FR-TKT-03 | GET | /api/work/tickets/{id} | Chi tiết nội bộ theo quyền. |
| FR-TKT-04 | GET | /api/tickets/{id}/history | Audit theo thời điểm rồi khóa. |
| FR-OPS-01 | POST | /api/tickets/{id}/claim | version, operation_id; assignee từ phiên. |
| FR-OPS-02 | POST | /api/tickets/{id}/start | version, operation_id. |
| FR-OPS-03 | POST | /api/tickets/{id}/progress | content, version, operation_id. |
| FR-OPS-04 | POST | /api/tickets/{id}/resolve | resolution, version, operation_id. |
| FR-OPS-05 | POST | /api/tickets/{id}/reject | rejection_reason, version, operation_id. |
| FR-ASG-01 | POST | /api/tickets/{id}/assign | assignee_id, version, operation_id. |
| FR-ASG-02 | POST | /api/tickets/{id}/reassign | assignee_id, reason, version, operation_id. |
| FR-ASG-03 | POST | /api/tickets/{id}/transfer | department_id, category_id, reason, version, operation_id. |
| FR-COM-01 | POST | /api/tickets/{id}/information-requests | content, version, operation_id. |
| FR-COM-02 | POST | /api/tickets/{id}/information-answers | information_request_id, content, version, operation_id. |
| FR-COM-03 | GET | /api/tickets/{id}/conversation | Câu hỏi và answer theo quyền. |
| FR-NOT-01 | Job | domain event processor | Nội bộ; event_id/recipient_id chống trùng. |
| FR-NOT-02 | GET | /api/my/notifications | filter, page; recipient từ phiên. |
| FR-NOT-03 | POST | /api/my/notifications/{id}/read | Ghi read_at nếu rỗng; CSRF. |
| FR-SLA-01 | Domain service | Trong tạo Ticket | Tính hạn trước commit; không là API cho user đặt SLA. |
| FR-SLA-02 | POST | /api/tickets/{id}/due-date | due_at, reason, version, operation_id. |
| FR-SLA-03 | Query và job | overdue evaluator | is_overdue khi đọc; quét và event theo ticket_id/due_at. |
| FR-FDB-01 | POST | /api/tickets/{id}/close | version, operation_id; chủ từ phiên. |
| FR-FDB-02 | POST | /api/tickets/{id}/feedback | rating, comment, version, operation_id. |
| FR-RPT-01 | GET | /api/reports/status | Bộ lọc báo cáo chuẩn. |
| FR-RPT-02 | GET | /api/reports/overdue | Bộ lọc chuẩn; page từ 1, 20 dòng. |
| FR-RPT-03 | GET | /api/reports/resolution-time | Bộ lọc chuẩn; outcome RESOLVED. |
| FR-RPT-04 | GET | /api/reports/categories | Bộ lọc chuẩn; loại hiện tại. |
| FR-RPT-05 | GET | /api/reports/workload | Bộ lọc chuẩn; ba trạng thái mở. |
| FR-RPT-06 | GET | /api/reports/satisfaction | Bộ lọc chuẩn; feedback sau CLOSED. |


Đây là contract đề nghị để FE/BE cùng triển khai, không là mô tả API đã chạy. Endpoint trả JSON cùng correlation_id. Ghi Ticket thành công trả code, ticket_id, status, version và thông tin tối thiểu được phép; không trả dữ liệu ngoài quyền hiện tại. Sau chuyển phòng ban, QL nguồn chỉ nhận xác nhận chuyển và mã Ticket, không nhận chi tiết đích nếu không được quản lý đích.

Lỗi chuẩn: 401 AUTH_REQUIRED khi không có phiên; 403 ACTION_FORBIDDEN khi sai vai trò; 404 TICKET_NOT_FOUND cho Ticket không tồn tại hoặc ngoài phạm vi; 422 VALIDATION_ERROR cho dữ liệu/bộ lọc sai; 409 VERSION_CONFLICT, STATE_CONFLICT hoặc OPERATION_PAYLOAD_CONFLICT cho điều kiện không còn hợp lệ; 500 INTERNAL_ERROR có mã chẩn đoán, không stack trace. Đăng nhập sai trả 401 với thông báo chung. Tạo mới trả 201; retry của tạo hoặc ghi trả kết quả đã commit với 200 và replayed true, không ghi lại.

Trường ghi không được phép như student_id, role, status hoặc created_at không được dùng để thay dữ liệu máy chủ. Một bộ lọc phòng ban ngoài quyền bị từ chối; không âm thầm mở rộng hoặc trả dữ liệu đó. Response lỗi trường có field và message để FE gắn đúng ô nhập. List trả items, page, page_size, total, total_pages; tất cả dùng cùng điều kiện quyền/lọc trong một lần đọc.

Đầu vào báo cáo là from_date, to_date và department_id tùy chọn. Từ 00:00 from_date đến trước 00:00 ngày sau to_date theo Asia/Ho_Chi_Minh, chuyển sang UTC để truy vấn. Bộ lọc luôn theo created_at; trạng thái, loại, phòng ban, assignee và hạn là hiện hành. Report trả filters, as_of_at, count/sample_count và metric. Trung bình/tỷ lệ không mẫu là null; không thay bằng 0. Hiển thị làm tròn một số lẻ, giá trị tính chưa làm tròn có thể giữ trong response.

[Về danh mục PRD](../README.md)
