# Thiết kế bảo mật và kiểm quyền

Người dùng được xác định từ phiên server. Controller không tin user_id, role hoặc phạm vi do trình duyệt gửi. Mọi truy vấn Ticket xây điều kiện quyền trước lấy nội dung; lỗi không lộ Ticket ngoài phạm vi. Mọi thao tác ghi kiểm lại quyền, status, assignee và version trong cùng transaction, kể cả khi UI đã ẩn nút hoặc người dùng có form đang mở.

Mật khẩu dùng cơ chế hash được hỗ trợ bởi runtime PHP; không lưu thô. Phiên thay mã sau xác thực, cookie HttpOnly và SameSite, Secure khi HTTPS; thời gian không hoạt động tối đa 30 phút. Mọi request ghi từ browser có CSRF token; logout cũng vô hiệu phiên ở máy chủ. Truy vấn tham số và escape nội dung khi hiển thị để chống dữ liệu nhập thực thi mã.

operation_id và payload_hash thuộc actor/action. Retry đã commit của chính actor có thể nhận xác nhận tối thiểu dù sau đó đã mất quyền đọc Ticket; không trả nội dung ngoài quyền hiện tại và không thực hiện thêm thay đổi. Request mới hoặc operation_id của người khác vẫn kiểm toàn bộ quyền hiện hành. Không dùng idempotency làm cách bỏ qua quyền hoặc đọc lại response chứa nội dung nhạy cảm.

Job nội bộ không được gọi tùy ý từ một endpoint người dùng; lệnh chạy cần cấu hình môi trường phù hợp. Recipient snapshot chỉ gồm account id, notification không chứa nội dung Ticket. Log giới hạn field theo NFR-OBS-01. Database credential nằm ở environment, không trong source hoặc seed công khai. Tài liệu bàn giao chỉ dùng dữ liệu giả lập và hướng dẫn cấu hình bí mật, không điền token thật.

[Về danh mục PRD](../README.md)
