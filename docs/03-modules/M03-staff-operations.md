# Module 03: Điều phối và Xử lý (Staff Operations)

### [D1] Phân công yêu cầu
**Mô tả:** Quản lý điều phối Ticket cho nhân viên.
- **AC-D1.1:** Giao diện Quản lý nhận diện rõ Ticket "Chưa phân công". Một Ticket tại một thời điểm chỉ có tối đa 01 người phụ trách (Assignee).
- **AC-D1.2:** Khi đổi người phụ trách, hệ thống lưu lịch sử: Người cũ, Người mới, Người thao tác đổi, Thời điểm đổi. Quyền thao tác lập tức chuyển sang người mới.

### [D2] Xử lý và Lịch sử
**Mô tả:** Nhân viên cập nhật tiến độ theo quy trình.
- **AC-D2.1:** Bấm "Giải quyết" nhưng để trống kết quả -> Báo lỗi chặn luồng. Chuyển trạng thái nhảy cóc (sai vòng đời quy định) -> DB từ chối.
- **AC-D2.2:** Mỗi lần chuyển trạng thái, Audit Log phải lưu đầy đủ (Trạng thái trước, Trạng thái sau, Người làm, Thời gian) mà không ghi đè dữ liệu cũ.