# NFR-REL-01 — Nhất quán và gửi lặp

Nghiệp vụ/lịch sử cùng transaction; kiểm tra phiên bản; gửi lặp không tạo hai yêu cầu/kết quả.

## Cách kiểm tra

Giả lỗi trước commit, gửi lặp và hai cập nhật cùng phiên bản; expected rollback hoặc một kết quả hợp lệ, không ghi đè.

Mã thực thi: `NFR-REL-01`. Result: Not Run. Evidence: chưa có. Chủ chuẩn bị QA, BE/FE hỗ trợ theo tiêu chí. Lỗi chưa xử lý phải nêu trong handoff.
