# Thiết kế bảo vệ thông tin

Tất cả route dùng quyền máy chủ; query dữ liệu đã giới hạn phạm vi, không lấy toàn bộ rồi chỉ lọc UI. Sinh viên chỉ thấy nội dung công khai của hồ sơ mình. Nhân viên chỉ sửa hồ sơ hiện đang được giao. Quyền điều phối/quản lý giới hạn đơn vị đã seed. Nếu sau này thêm tài liệu, tải tài liệu cũng phải kiểm tra quyền hồ sơ, không public URL bỏ qua auth.

PHP session đổi ID sau login, hết hạn theo cấu hình và vô hiệu khi logout; cookie cấu hình an toàn theo môi trường. Dùng password_hash/password_verify, prepared statements, output escaping, CSRF cho POST, secret ngoài repository. Seed và evidence chỉ dữ liệu giả lập. Test NFR-SEC-01/E2E-06 phải kiểm tra HTML và JSON trả về.
