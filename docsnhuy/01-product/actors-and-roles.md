# Vai trò và phạm vi quyền

| Vai trò | Phạm vi đọc | Phạm vi hành động |
| --- | --- | --- |
| SV Sinh viên | Chỉ Ticket do mình tạo; thông báo của mình | Tạo Ticket; trả lời bổ sung; đóng Ticket đã có kết quả; đánh giá sau đóng. |
| NV Nhân viên | Ticket hiện được giao; hàng chờ RECEIVED chưa phân công trong phòng ban mình | Nhận từ hàng chờ; bắt đầu việc được giao; cập nhật tiến độ; yêu cầu bổ sung; hoàn tất hoặc từ chối. |
| QL Quản lý phòng ban | Ticket hiện thuộc phòng ban được quản lý; báo cáo cùng phạm vi | Phân loại; phân công; đổi người; chuyển phòng ban; điều chỉnh hạn. Không xử lý thay NV. |
| Hệ thống | Dữ liệu cần cho tác vụ nội bộ, không là tài khoản dùng giao diện | Kiểm quyền; tính hạn; quét quá hạn; tạo thông báo theo sự kiện. |


Một tài khoản có một vai trò nghiệp vụ. NV thuộc một phòng ban; QL có tập phòng ban được quản lý. Mỗi phòng ban hoạt động trong bộ thử có ít nhất một QL hoạt động. Tài khoản, quyền, phòng ban, loại vấn đề và SLA được nạp sẵn bằng seed dữ liệu. PM, FE, BE và QA là vai trò thực hiện dự án, không tự trở thành vai trò ứng dụng.

Sau phân công lại, NV cũ mất quyền đọc và xử lý Ticket. Sau chuyển phòng ban, quyền được tính theo phòng ban đích; QL nguồn chỉ còn quyền nếu cũng được cấp quản lý đích. SV vẫn đọc Ticket của mình xuyên suốt. Lịch sử hiển thị tên actor và vai trò tại thời điểm sự kiện; không hiển thị email, mật khẩu, mã phiên hoặc danh sách quyền quản trị.

[Về danh mục PRD](../README.md)
