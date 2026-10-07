# Bảng chuyển trạng thái

| Mã | Trước | Sau | FR | Actor | Điều kiện |
| --- | --- | --- | --- | --- | --- |
| ST-01 | Chưa có | RECEIVED | FR-STU-02 | SV | Dữ liệu hợp lệ; chưa phân công; tạo hạn. |
| ST-02 | RECEIVED | PROCESSING | FR-OPS-01 | NV | Chưa phân công; nhận nguyên tử và gán mình. |
| ST-03 | RECEIVED | PROCESSING | FR-OPS-02 | NV | Đã được phân công cho NV trong phiên. |
| ST-04 | PROCESSING | WAITING_INFO | FR-COM-01 | NV | Lưu một câu hỏi OPEN. |
| ST-05 | WAITING_INFO | PROCESSING | FR-COM-02 | SV | Trả lời hợp lệ cho câu hỏi OPEN của mình. |
| ST-06 | RECEIVED hoặc PROCESSING | RECEIVED | FR-ASG-03 | QL | Chuyển phòng ban; loại đích hợp lệ; xóa người phụ trách. |
| ST-07 | PROCESSING | RESOLVED | FR-OPS-04 | NV | Có kết quả; không còn bổ sung OPEN. |
| ST-08 | PROCESSING | REJECTED | FR-OPS-05 | NV | Có lý do; không còn bổ sung OPEN. |
| ST-09 | RESOLVED hoặc REJECTED | CLOSED | FR-FDB-01 | SV | Chủ Ticket xác nhận đóng; giữ outcome. |


Mọi chuyển không có trong bảng đều bị từ chối ở máy chủ và không tạo lịch sử thay đổi trạng thái. Phân loại, phân công lần đầu, đổi người, cập nhật tiến độ, đổi hạn, tạo thông báo, đánh dấu đã đọc và gửi feedback không chuyển trạng thái. ST-06 có thể là RECEIVED sang RECEIVED nhưng vẫn là thay đổi phòng ban và trách nhiệm; phải ghi sự kiện chuyển phòng ban, không giả lập việc bắt đầu xử lý.

Một yêu cầu ghi chỉ được chấp nhận khi role, phạm vi, assignee, status và version cùng còn hợp lệ tại thời điểm giao dịch. Kiểm ở UI giúp người dùng chọn đúng thao tác nhưng không thay kiểm ở máy chủ.

[Về danh mục PRD](../README.md)
