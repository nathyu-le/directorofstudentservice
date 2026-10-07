# WF-01 Sinh viên gửi yêu cầu hỗ trợ

**Actor:** SV; hệ thống; QL tiếp nhận

**Preconditions:** SV đăng nhập; loại, phòng ban, SLA và QL hoạt động trong dữ liệu mẫu.

## Luồng chính

1. FR-STU-02 — SV điền form, gửi với operation_id; kiểm dữ liệu và quyền.
2. FR-SLA-01 — Trong giao dịch tạo, tính hạn từ SLA của loại.
3. FR-STU-02 — Commit Ticket RECEIVED chưa phân công, sự kiện và kết quả chống trùng.
4. FR-NOT-01 — Sau commit tạo thông báo cho SV và QL; retry không tạo trùng.
5. FR-STU-01, FR-STU-03 — SV tra cứu Ticket vừa tạo; quyền và trạng thái cùng dữ liệu máy chủ.

## Kết quả cuối

Một Ticket RECEIVED, mã duy nhất, hạn hợp lệ, chưa phân công; SV nhận mã và QL thấy hàng chờ.

## Luồng thay thế và lỗi

Dữ liệu sai hoặc lỗi lưu: không tạo bất cứ phần nghiệp vụ nào. Retry cùng operation_id trả mã cũ. Lỗi notification sau commit không làm Ticket mất.

## Kiểm thử đầu cuối

E2E-01, E2E-02.

[Về danh mục PRD](../README.md)
