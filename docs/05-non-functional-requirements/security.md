# Bảo mật

## NFR-SEC-01 Xác thực và phiên

**Yêu cầu:** Mật khẩu chỉ lưu dạng hash; cookie phiên HttpOnly, SameSite; môi trường nghiệm thu truy cập từ xa dùng HTTPS và Secure. Đổi mã phiên khi đăng nhập và vô hiệu ở máy chủ khi đăng xuất hoặc quá 30 phút không hoạt động.

**Cách kiểm chứng:** Kiểm cấu hình cookie, dữ liệu database, phiên trước và sau đăng nhập, replay phiên cũ; không đưa mật khẩu hoặc cookie vào bằng chứng chia sẻ.

## NFR-SEC-02 Quyền và dữ liệu nhập

**Yêu cầu:** Mọi endpoint đọc/ghi, báo cáo và job thay đổi dữ liệu thực thi quyền cần thiết; form ghi chống CSRF; truy vấn dùng tham số; dữ liệu văn bản escape khi hiển thị.

**Cách kiểm chứng:** Thử sai chủ, sai vai trò, sửa khóa phòng ban, CSRF token thiếu/sai, chuỗi SQL và script; không lộ dữ liệu hoặc thực thi mã, không có ghi ngoài quyền.

## NFR-SEC-03 Bảo vệ bí mật

**Yêu cầu:** Source, log, notification và dữ liệu bàn giao không chứa mật khẩu thô, token truy cập, cookie phiên hoặc dữ liệu cá nhân thật.

**Cách kiểm chứng:** QA rà repository, mẫu log và file seed trước bàn giao; bằng chứng chỉ dùng tài khoản và nội dung giả lập.

[Về danh mục PRD](../README.md)
