# Mục tiêu và nội dung ngoài mục tiêu

| Mã | Mục tiêu | Cách đo tại nghiệm thu |
| --- | --- | --- |
| GOAL-01 | Ít nhất 90% Ticket được giao rõ người phụ trách | Trong bộ thử đã chốt, đếm Ticket có assignee hợp lệ / tổng Ticket × 100; đo sau lượt tiếp nhận. |
| GOAL-02 | Ít nhất 90% Ticket được theo dõi đúng trạng thái | Đếm Ticket có trạng thái đúng trên danh sách và chi tiết của SV / tổng Ticket bộ thử × 100. |
| GOAL-03 | Giảm ít nhất 30% lượt hỏi tiến độ | (Lượt hỏi trước − lượt hỏi sau) / lượt hỏi trước × 100; hai kịch bản cùng nội dung, thời lượng và số Ticket; lượt trước phải lớn hơn 0. |
| GOAL-04 | Ngăn xử lý trùng và bảo vệ dữ liệu | Các ca retry, claim đồng thời, sai chủ và sai quyền bắt buộc đạt; không dùng ngưỡng 90% để chấp nhận lỗi này. |
| GOAL-05 | Số liệu quản lý đối chiếu được | Sáu báo cáo khớp kết quả tính độc lập trên bộ dữ liệu QA, bao gồm Ticket đã CLOSED và outcome. |


Các mục tiêu GOAL-01 đến GOAL-03 là ngưỡng kiểm chứng prototype, không cam kết thay đổi vận hành production. Bộ thử và cách đo phải được PM và Sponsor thống nhất trước UAT. Nếu chưa có dữ liệu so sánh lượt hỏi tiến độ, GOAL-03 được ghi Chưa đủ dữ liệu, không tự coi là đạt.

Không xây dựng production toàn trường, quản lý toàn bộ nghiệp vụ học vụ hoặc thực hiện hành động nghiệp vụ ngoài hệ thống. Không có tích hợp thật SSO, email, SMS, hệ thống trường; không thanh toán, tải tệp, chat tự do, đăng ký tài khoản, khôi phục mật khẩu hoặc quản trị danh mục bằng giao diện. Không tự mở lại, tự đóng theo thời gian hay sửa đánh giá đã gửi. Dữ liệu cá nhân thật không thuộc bộ nghiệm thu prototype.

[Về danh mục PRD](../README.md)
