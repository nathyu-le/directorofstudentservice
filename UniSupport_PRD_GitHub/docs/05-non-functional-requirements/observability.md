# NFR-OBS-01 — Truy vết lỗi và nghiệp vụ

Có request/correlation ID cho lỗi; lịch sử nghiệp vụ có tác nhân/thời gian/hành động, không lộ mật khẩu hay token.

## Cách kiểm tra

Tạo lỗi có kiểm soát và cập nhật hồ sơ; đối chiếu log với sự kiện; tìm dấu mật khẩu/token mẫu, expected không có.

Mã thực thi: `NFR-OBS-01`. Result: Not Run. Evidence: chưa có. Chủ chuẩn bị QA, BE/FE hỗ trợ theo tiêu chí. Lỗi chưa xử lý phải nêu trong handoff.
