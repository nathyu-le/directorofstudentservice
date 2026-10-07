# WF-05 Ghi kết quả giải quyết hoặc từ chối

**Actor:** NV phụ trách; SV

**Preconditions:** Ticket PROCESSING, không câu hỏi OPEN.

## Luồng chính

1. FR-OPS-04 — Nhánh thành công: NV nhập resolution và chuyển RESOLVED, outcome RESOLVED.
2. FR-OPS-05 — Nhánh từ chối: NV nhập rejection_reason và chuyển REJECTED, outcome REJECTED.
3. FR-OPS-04 hoặc FR-OPS-05 — Trong giao dịch ghi ended_at, version và sự kiện; FR-TKT-04 đọc lịch sử.
4. FR-NOT-01, FR-STU-03 — SV nhận thông báo và đọc kết quả. Chưa tạo closed_at.

## Kết quả cuối

Có đúng một kết quả xử lý, ended_at hợp lệ; Ticket chỉ đọc đối với nghiệp vụ xử lý.

## Luồng thay thế và lỗi

Thiếu kết quả/lý do, sai người hoặc WAITING_INFO: không kết thúc. Không dùng từ chối thay chuyển phòng ban khi yêu cầu có thể được tiếp nhận ở phòng ban khác. Retry không đổi ended_at.

## Kiểm thử đầu cuối

E2E-07, E2E-08.

[Về danh mục PRD](../README.md)
