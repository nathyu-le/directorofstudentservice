# Giả định nghiệp vụ và vận hành

| Mã | Giả định | Ảnh hưởng khi thay đổi |
| --- | --- | --- |
| ASM-01 | Prototype dùng tài khoản, phòng ban, danh mục và SLA nạp sẵn | Nếu cần quản trị bằng UI, thêm FR và ước lượng; không gộp vào đăng nhập. |
| ASM-02 | Một tài khoản một vai trò; NV một phòng ban; QL có tập phòng ban | Đa vai trò hoặc NV nhiều phòng ban ảnh hưởng quyền, hàng chờ và claim. |
| ASM-03 | SLA giờ liên tục, mặc định dữ liệu mẫu 48 giờ; không dừng khi chờ | Lịch làm việc, ngày nghỉ hoặc pause cần mô hình và test mới. |
| ASM-04 | QL nguồn có quyền chuyển tới đích hoạt động mà không cần phê duyệt đích | Cơ chế nhận/chấp nhận chuyển sẽ thêm trạng thái và workflow. |
| ASM-05 | SV xác nhận đóng, đánh giá một lần sau CLOSED; không tự đóng/mở lại | Chính sách khác ảnh hưởng trạng thái, feedback và báo cáo. |
| ASM-06 | Thông báo nội ứng dụng bằng xử lý event sau commit; job chạy được trong môi trường demo | Email, SMS hoặc realtime push cần tích hợp và yêu cầu vận hành mới. |


Các giả định đã được áp dụng nhất quán trong đặc tả này để có hành vi kiểm thử được. Sponsor cần xác nhận tại review baseline; không để các nhóm tự chọn cách khác nhau khi triển khai. Khi thay giả định, PM lập tác động đến FR, AC, workflow, kiến trúc và kế hoạch.

[Về danh mục PRD](../README.md)
