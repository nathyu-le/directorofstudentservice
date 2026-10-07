# NFR-PERF-01 — Đáp ứng đủ dùng cho demo

Mục tiêu thử đề xuất: 95% trong 20 lần đo trang danh sách/chi tiết/báo cáo ≤3 giây với 1.000 yêu cầu mẫu trên máy demo ghi rõ cấu hình. Chưa là SLA production.

## Cách kiểm tra

Seed 1.000 hồ sơ tổng hợp; ghi máy, môi trường, thời gian từng lần, p95; không so trên máy khác mà bỏ cấu hình.

Mã thực thi: `NFR-PERF-01`. Result: Not Run. Evidence: chưa có. Chủ chuẩn bị QA, BE/FE hỗ trợ theo tiêu chí. Lỗi chưa xử lý phải nêu trong handoff.
