# WF-04 Chuyển phòng ban và tiếp nhận lại

**Actor:** QL nguồn; QL đích; NV đích; SV

**Preconditions:** Ticket RECEIVED hoặc PROCESSING, không câu hỏi OPEN; phòng ban và loại đích hoạt động, có QL đích.

## Luồng chính

1. FR-TKT-03 — QL nguồn xác nhận Ticket thuộc phạm vi mình quản lý.
2. FR-ASG-03 — Chọn đích, loại đích, lý do; đổi cùng giao dịch và xóa phân công cũ, chuyển RECEIVED.
3. FR-IAM-03, FR-NOT-01 — Quyền tính theo đích; thông báo SV, người cũ và QL hai bên theo bảng định tuyến.
4. FR-TKT-01 — Ticket xuất hiện ở hàng chờ đích, rời phạm vi nguồn nếu QL nguồn không quản lý đích.
5. FR-OPS-01 hoặc FR-ASG-01 và FR-OPS-02 — NV đích tự nhận, hoặc QL đích giao rồi NV bắt đầu.

## Kết quả cuối

Giữ mã, chủ, nội dung gốc, lịch sử và hạn; trách nhiệm mới được xác lập tại đích.

## Luồng thay thế và lỗi

WAITING_INFO, đích sai loại hoặc không có QL hoạt động: không chuyển. Kết quả được lưu đồng thời với chuyển: version quyết định một giao dịch thắng. Người cũ không ghi bằng form cũ.

## Kiểm thử đầu cuối

E2E-06.

[Về danh mục PRD](../README.md)
