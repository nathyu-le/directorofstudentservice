# Danh mục module và chức năng

| Module | Tên | Số FR | Ranh giới |
| --- | --- | --- | --- |
| M01 | Danh tính và quyền truy cập | 3 | Xác thực phiên và kiểm tra quyền; không tạo hay xử lý Ticket. |
| M02 | Yêu cầu của sinh viên | 3 | Tạo Ticket và theo dõi Ticket của chính sinh viên; không điều phối hoặc xử lý thay nhân viên. |
| M03 | Quản lý Ticket | 4 | Tra cứu, phân loại và đọc lịch sử Ticket; không phân công hay đổi trạng thái xử lý. |
| M04 | Nghiệp vụ xử lý của nhân viên | 5 | Nhận việc, bắt đầu, cập nhật tiến độ và ghi kết quả xử lý. |
| M05 | Phân công và phòng ban | 3 | Đổi trách nhiệm xử lý bằng phân công, đổi người hoặc chuyển phòng ban. |
| M06 | Trao đổi thông tin | 3 | Yêu cầu bổ sung, trả lời và đọc hội thoại gắn với Ticket. |
| M07 | Thông báo | 3 | Tạo, đọc và đánh dấu thông báo trong ứng dụng; không gửi email hoặc SMS. |
| M08 | Hạn xử lý | 3 | Tính hạn, điều chỉnh hạn và xác định quá hạn; không tự đổi trạng thái Ticket. |
| M09 | Đóng Ticket và phản hồi | 2 | Sinh viên xác nhận đóng Ticket và gửi một đánh giá sau khi đóng. |
| M10 | Báo cáo | 6 | Tổng hợp số liệu theo cùng bộ lọc và phạm vi quyền; không ghi thay đổi Ticket. |


Module là nhóm trách nhiệm nghiệp vụ, không là một chức năng tổng hợp. Mỗi FR bên trong biểu diễn một hành vi riêng có actor, dữ liệu, điều kiện, hiệu ứng và tiêu chí nghiệm thu. Một workflow đi qua nhiều module dùng lại FR, không sao chép một chức năng vào nhiều module. Chức năng đọc chi tiết tổng hợp dữ liệu có liên kết tới hội thoại/lịch sử, không tự tạo một nghiệp vụ ghi khác.

[Về danh mục PRD](../README.md)
