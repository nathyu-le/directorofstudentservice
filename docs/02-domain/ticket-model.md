# Mô hình dữ liệu Ticket nghiệp vụ

| Dữ liệu | Ý nghĩa và ràng buộc |
| --- | --- |
| ticket_id và code | Khóa kỹ thuật và mã hiển thị duy nhất, bất biến suốt vòng đời. |
| student_id | Chủ Ticket lấy từ phiên, bất biến. |
| category_id, department_id | Cặp loại và phòng ban hiện tại hợp lệ; chuyển phòng ban đổi cả hai đồng thời. |
| title, description | Nội dung gốc, giới hạn ở FR-STU-02, không sửa sau gửi. |
| assignee_id | Có thể null; khi có phải là NV thuộc phòng ban hiện tại tại thời điểm giao. |
| status | Một trong sáu trạng thái; trạng thái kết thúc không thể được phân công hay tiếp tục xử lý. |
| sla_hours_snapshot, due_at | Snapshot nguyên 1 đến 168 và hạn bắt buộc; thay cấu hình sau tạo không sửa các Ticket cũ. |
| created_at, updated_at | Thời điểm máy chủ; created_at bất biến, updated_at đổi khi nghiệp vụ Ticket đổi. |
| started_at | Thời điểm bắt đầu đầu tiên; không đặt lại sau chuyển phòng ban. |
| ended_at, outcome | Cùng rỗng khi còn mở; có khi RESOLVED hoặc REJECTED và giữ khi CLOSED. |
| resolution, rejection_reason | Chỉ trường tương ứng outcome có nội dung kết quả; không dùng một trường thay cho hai nghĩa. |
| closed_at | Chỉ có khi CLOSED; không dùng để tính thời gian giải quyết. |
| version | Bắt đầu 1; tăng một cho mỗi thao tác nghiệp vụ ghi thành công. |
| is_overdue | Giá trị suy ra, không lưu để thay thế status. |
| information_requests và answers | Nhiều vòng hỏi đáp; một OPEN tại một thời điểm, một answer mỗi request. |
| progress_updates và audit_events | Nhiều cập nhật; lịch sử bất biến gắn cùng Ticket. |
| feedback | Không hoặc một; chỉ tạo sau CLOSED bởi SV chủ Ticket. |


RESOLVED yêu cầu outcome RESOLVED, ended_at và resolution; REJECTED yêu cầu outcome REJECTED, ended_at và rejection_reason. CLOSED yêu cầu closed_at và giữ outcome trước đó. Ticket mở không có ended_at, outcome, closed_at hoặc feedback. Phòng ban, loại, tài khoản đã được tham chiếu không xóa vật lý trong prototype; seed đổi active để kiểm thử. Không có giao diện chỉnh cấu hình.

[Về danh mục PRD](../README.md)
