# WF-02 Tiếp nhận và bắt đầu xử lý

**Actor:** NV hoặc QL

**Preconditions:** Ticket RECEIVED thuộc phòng ban hoạt động.

## Luồng chính

1. FR-TKT-01 — NV hoặc QL mở hàng chờ.
2. FR-OPS-01 — Nhánh A: NV nhận Ticket chưa phân công; gán NV và PROCESSING đồng thời.
3. FR-ASG-01 — Nhánh B: QL phân công NV; Ticket vẫn RECEIVED.
4. FR-OPS-02 — Nhánh B tiếp tục: NV được giao bắt đầu, chuyển PROCESSING.
5. FR-TKT-03, FR-OPS-03 — NV đọc chi tiết và ghi tiến độ công khai; SV theo dõi cập nhật.
6. FR-NOT-01 — Các sự kiện có trong bảng định tuyến tạo thông báo đúng người nhận.

## Kết quả cuối

Ticket PROCESSING, một người phụ trách, started_at lần đầu và lịch sử đúng nhánh.

## Luồng thay thế và lỗi

Hai NV cùng nhận hoặc nhận đồng thời với phân công: chỉ một giao dịch thắng. NV khác không xử lý thay. Phân công không tự bắt đầu.

## Kiểm thử đầu cuối

E2E-03, E2E-04.

[Về danh mục PRD](../README.md)
