# Module 04: Quản lý và Báo cáo (Reporting)

### [R1] Báo cáo Tình trạng & Thời gian
**Mô tả:** Quản lý xem thống kê tổng quan về tiến độ.
- **AC-R1.1:** Số lượng Ticket theo từng trạng thái trên Dashboard phải khớp 100% với dữ liệu thực tế truy vấn từ Database, không đếm trùng lặp.
- **AC-R1.2:** Cách tính SLA: Thời gian xử lý = [Thời điểm giải quyết] - [Thời điểm gửi]. Nếu hạn giả lập đã qua mà chưa có kết quả -> Đánh dấu "Quá hạn" (Màu đỏ).

### [R2] Phân tích Vấn đề & Tải việc
**Mô tả:** Báo cáo chi tiết theo phân loại và mức độ hài lòng.
- **AC-R2.1:** Bảng Tải công việc (Workload) hiển thị số lượng Ticket "Đang xử lý" của từng nhân viên. Không cộng dồn Ticket "Đã giải quyết" vào cột Workload hiện tại.
- **AC-R2.2:** Điểm hài lòng tính trung bình dựa trên các phản hồi hợp lệ (1-5 sao). Nếu Ticket chưa có đánh giá -> Bỏ qua khi tính trung bình (hiển thị "Chưa có dữ liệu", không tính là 0 điểm).