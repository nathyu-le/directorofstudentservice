# Vòng đời Ticket

| Trạng thái | Ý nghĩa | Việc được phép tiếp theo |
| --- | --- | --- |
| RECEIVED | Đã tiếp nhận; có thể chưa giao hoặc đã giao nhưng chưa bắt đầu | NV nhận hoặc bắt đầu; QL phân loại, phân công/đổi người, chuyển phòng ban, đổi hạn. |
| PROCESSING | NV hiện tại đang xử lý | NV ghi tiến độ, hỏi bổ sung, hoàn tất hoặc từ chối; QL phân loại, đổi người, chuyển phòng ban, đổi hạn. |
| WAITING_INFO | Một câu hỏi bổ sung OPEN đang chờ SV | SV trả lời; QL đổi người hoặc đổi hạn. Không chuyển phòng ban khi còn câu hỏi mở. |
| RESOLVED | Có kết quả giải quyết thành công | SV xác nhận đóng; mọi nghiệp vụ xử lý chỉ đọc. |
| REJECTED | Có lý do từ chối | SV xác nhận đóng; mọi nghiệp vụ xử lý chỉ đọc. |
| CLOSED | SV đã xác nhận đóng sau một kết quả | SV đánh giá một lần; còn lại chỉ đọc. |


Được đọc không đồng nghĩa được thực hiện mọi thao tác. Cùng một trạng thái, quyền còn phụ thuộc chủ Ticket, assignee và phòng ban. Không có tự động đổi trạng thái do quá hạn, xem trang hay gửi notification. Nội dung bổ sung đã trả lời và cập nhật tiến độ được giữ sau khi có kết quả; CLOSED không làm mất dữ liệu.

[Về danh mục PRD](../README.md)
