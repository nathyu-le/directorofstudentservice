# Phạm vi — proposal là nguồn gốc

| Mã | Module đúng proposal | Các hành vi triển khai |
| --- | --- | --- |
| STU | Cổng hỗ trợ sinh viên | Gửi yêu cầu hỗ trợ; Xem danh sách yêu cầu của tôi; Xem chi tiết và tiến độ yêu cầu; Bổ sung thông tin được yêu cầu; Phản hồi kết quả hỗ trợ |
| DSP | Điều phối và xử lý yêu cầu | Xem hàng chờ điều phối; Xem hồ sơ xử lý và lịch sử nghiệp vụ; Phân loại yêu cầu; Phân công trách nhiệm xử lý; Đặt hạn xử lý dự kiến; Bắt đầu xử lý yêu cầu; Yêu cầu sinh viên bổ sung thông tin; Ghi cập nhật tiến độ xử lý; Ghi kết quả giải quyết; Đóng yêu cầu đã giải quyết |
| RPT | Quản lý và báo cáo | Tổng hợp trạng thái yêu cầu; Theo dõi yêu cầu quá hạn; Thống kê thời gian xử lý; Thống kê vấn đề phổ biến; Thống kê khối lượng theo phòng ban; Tổng hợp mức hài lòng |
| IAM | Tài khoản và quyền truy cập | Đăng nhập; Đăng xuất; Kiểm soát quyền theo vai trò và hồ sơ |

## Nguyên tắc suy ra chức năng

Proposal mô tả năng lực, không ấn định số FR. 24 FR dưới đây là cách phân rã đề xuất để xây và kiểm thử; không coi số chức năng hay chi tiết thiết kế là nguyên văn proposal. Ví dụ “phân công” cần trường người/đơn vị và lịch sử đổi trách nhiệm; “quá hạn” cần hạn dự kiến; “hài lòng” cần phản hồi có điểm. Chúng ở trong 4 module gốc.

Không tách Ticket, SLA, Notification, Knowledge Base, Attachment, Admin thành module sản phẩm. Không triển khai mở lại tự động, email/SMS, upload tệp, quy trình phê duyệt chuyển giao nhiều cấp hoặc quản trị danh mục/tài khoản khi proposal chưa xác nhận. Tài liệu liên quan được bảo vệ theo quyền; cơ chế upload là OQ-04, chưa là FR giao làm.

Website cần dùng trên máy tính/điện thoại. Thiết kế xem xét tiếng Việt/Anh theo yêu cầu bài tập; mức hoàn thiện song ngữ cần xác nhận, không tuyên bố đã có production song ngữ.
