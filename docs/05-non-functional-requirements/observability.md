# Khả năng quan sát

## NFR-OBS-01 Log để điều tra

**Yêu cầu:** Log có correlation_id, action, actor_id nếu có, ticket_id, kết quả và thời điểm; lỗi có mã chẩn đoán. Không log password, token hay toàn bộ nội dung Ticket.

**Cách kiểm chứng:** Từ một lỗi giao dịch và một retry notification, BE/QA tìm được đường xử lý qua correlation_id; kiểm mẫu log không chứa bí mật.

## NFR-OBS-02 Quan sát tác vụ nền

**Yêu cầu:** Có thông tin số event chờ, lỗi retry gần nhất và thời điểm job SLA chạy; dùng log hoặc lệnh kiểm tra nội bộ, không yêu cầu dashboard quản trị mới.

**Cách kiểm chứng:** Dừng job, kiểm dấu hiệu ngừng; phục hồi và đối chiếu hàng đợi về hết phần đã xử lý. Ghi mốc chạy và số lượng trước/sau.

[Về danh mục PRD](../README.md)
