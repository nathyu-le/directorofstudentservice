# NFR-SEC-01 — Bảo vệ phiên, quyền và dữ liệu

Mọi API kiểm tra phiên/vai trò/đối tượng; mật khẩu băm; truy vấn tham số hóa; output encode; chống CSRF cho thao tác ghi; không ghi mật khẩu/nội dung nhạy cảm vào log.

## Cách kiểm tra

Chạy ma trận quyền, giả role/id, SQL injection/XSS, CSRF thiếu/sai; expected: bị chặn hoặc hiển thị an toàn, không thay dữ liệu.

Mã thực thi: `NFR-SEC-01`. Result: Not Run. Evidence: chưa có. Chủ chuẩn bị QA, BE/FE hỗ trợ theo tiêu chí. Lỗi chưa xử lý phải nêu trong handoff.
