# Module 01: Tài khoản và Quyền truy cập (Identity & Access)

### [A1] Đăng nhập hệ thống
**Mô tả:** Người dùng xác thực tài khoản thử nghiệm để truy cập hệ thống.
- **AC-A1.1:** Nhập đúng thông tin hợp lệ -> Chuyển hướng đúng giao diện theo Role (SV, NV, QL). Sai thông tin -> Hiển thị lỗi từ chối.
- **AC-A1.2:** Sau khi đăng xuất, cố tình truy cập lại URL bên trong bằng lịch sử trình duyệt -> Bị chặn và đẩy về trang đăng nhập.

### [A2] Quản lý Phân quyền (RBAC)
**Mô tả:** Hệ thống kiểm soát quyền đọc/ghi dữ liệu theo vai trò ở cấp độ API và giao diện.
- **AC-A2.1 (Data Isolation):** Sinh viên A không thể xem Ticket của Sinh viên B (kể cả khi gõ trực tiếp URL/ID). Nhân viên không thể sửa/cập nhật Ticket không được phân công cho mình.
- **AC-A2.2 (Scope Management):** Quản lý (QL) chỉ xem và phân công Ticket thuộc phòng ban của mình.