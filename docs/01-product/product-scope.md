# Phạm vi sản phẩm và kiểm soát thay đổi

| Module | Tên | Trách nhiệm riêng |
| --- | --- | --- |
| M01 | Danh tính và quyền truy cập | Xác thực phiên và kiểm tra quyền; không tạo hay xử lý Ticket. |
| M02 | Yêu cầu của sinh viên | Tạo Ticket và theo dõi Ticket của chính sinh viên; không điều phối hoặc xử lý thay nhân viên. |
| M03 | Quản lý Ticket | Tra cứu, phân loại và đọc lịch sử Ticket; không phân công hay đổi trạng thái xử lý. |
| M04 | Nghiệp vụ xử lý của nhân viên | Nhận việc, bắt đầu, cập nhật tiến độ và ghi kết quả xử lý. |
| M05 | Phân công và phòng ban | Đổi trách nhiệm xử lý bằng phân công, đổi người hoặc chuyển phòng ban. |
| M06 | Trao đổi thông tin | Yêu cầu bổ sung, trả lời và đọc hội thoại gắn với Ticket. |
| M07 | Thông báo | Tạo, đọc và đánh dấu thông báo trong ứng dụng; không gửi email hoặc SMS. |
| M08 | Hạn xử lý | Tính hạn, điều chỉnh hạn và xác định quá hạn; không tự đổi trạng thái Ticket. |
| M09 | Đóng Ticket và phản hồi | Sinh viên xác nhận đóng Ticket và gửi một đánh giá sau khi đóng. |
| M10 | Báo cáo | Tổng hợp số liệu theo cùng bộ lọc và phạm vi quyền; không ghi thay đổi Ticket. |


Tất cả 35 chức năng trong danh mục là Must của baseline đề nghị. Không coi một module đã xong khi mới hoàn thành một màn hình mà thiếu quy tắc, trường hợp lỗi hoặc tiêu chí bắt buộc. Kiến trúc và ADR là thiết kế hỗ trợ các FR, không được tự tạo chức năng sản phẩm ngoài danh mục.

Khung dự án là 15 tuần với 288 giờ thực hiện và 32 giờ dự phòng, tổng 320 giờ. Đây là ràng buộc kế hoạch; PRD không tự chứng minh phạm vi mới khả thi trong khung đó. Master Plan phải ước lượng từng FR và các việc chung, đối chiếu năng lực của bốn vai trò PM/PO, BE, FE, QA. Nếu vượt khung, PM trình quyết định đổi phạm vi hoặc nguồn lực; không âm thầm bỏ test hay tiêu chí.

So với danh mục 25 chức năng trước, bản này phân tách theo 10 module và đưa vào các hành vi của mẫu mới: tự nhận Ticket, từ chối, chuyển phòng ban, đóng Ticket, hội thoại, thông báo và SLA mặc định. Mã cũ chỉ dùng trong bảng đối chiếu; không dùng mã A01/S01/D01/R01 để lập task mới. Thời điểm đánh giá được thống nhất sau CLOSED; trạng thái mở gồm RECEIVED, PROCESSING, WAITING_INFO.

Khi FR, tên, rule, trạng thái hoặc AC thay đổi, PM cập nhật PRD, matrix và manifest trước, sau đó cập nhật Team Charter nếu quy trình bị ảnh hưởng và Master Plan nếu công việc, phụ thuộc hoặc thời gian thay đổi. Một thay đổi cần ghi lý do, người xác nhận, phạm vi tác động và phiên bản. Không tự coi bản này đã được Sponsor phê duyệt.

[Về danh mục PRD](../README.md)
