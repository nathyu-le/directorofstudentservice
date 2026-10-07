# Module 02: Cổng hỗ trợ Sinh viên (Student Portal)

### [S1] Gửi và Bổ sung yêu cầu
**Mô tả:** Sinh viên tạo yêu cầu mới hoặc bổ sung thông tin cho yêu cầu đang chờ.
- **AC-S1.1:** Bỏ trống trường bắt buộc (Loại yêu cầu, Nội dung) -> Báo lỗi tại trường đó. Nhập hợp lệ -> Tạo Ticket, sinh mã Ticket duy nhất (VD: SUP-1001), lưu thời gian.
- **AC-S1.2:** Chức năng bổ sung thông tin (khi trạng thái là "Chờ bổ sung") phải ghi nối tiếp vào đúng Ticket hiện tại, tuyệt đối không tạo bản ghi Ticket mới.

### [S2] Theo dõi và Phản hồi
**Mô tả:** Sinh viên xem danh sách, trạng thái và đánh giá hài lòng.
- **AC-S2.1:** Danh sách hiển thị đúng trạng thái hiện hành. Nếu trạng thái là "Chờ bổ sung", phải highlight nội dung nhân viên yêu cầu bổ sung.
- **AC-S2.2:** Form đánh giá (Phản hồi) chỉ xuất hiện ở Ticket có trạng thái "Đã giải quyết". Sau khi submit đánh giá, dữ liệu lưu trữ vĩnh viễn (tải lại trang không mất).