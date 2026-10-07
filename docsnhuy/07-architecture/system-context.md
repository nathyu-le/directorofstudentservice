# Ngữ cảnh hệ thống

| Thành phần | Trách nhiệm | Ranh giới |
| --- | --- | --- |
| Trình duyệt SV/NV/QL | Hiển thị UI và gửi dữ liệu được phép | Không tự xác định actor, trạng thái, quyền hoặc due_at tự động. |
| Ứng dụng UniSupport | Phiên, quyền, hành vi Ticket, truy vấn và transaction | Một ứng dụng PHP; endpoint nội bộ cùng quy tắc máy chủ. |
| MySQL | Lưu Ticket, quan hệ, lịch sử, idempotency và event | Dữ liệu giả lập; transaction và khóa duy nhất bảo toàn nghiệp vụ. |
| Job notification và SLA | Xử lý event và quét hạn theo chu kỳ | Cùng codebase và database; không yêu cầu dịch vụ bên ngoài. |
| PM/QA/Sponsor | Review, thử nghiệm và xác nhận nghiệm thu | Dùng môi trường test/demo, không là kết nối hệ thống học vụ. |


Không có ranh giới tích hợp thật với SSO, email, SMS hoặc cơ sở dữ liệu trường. Khi nghiệm thu, trình duyệt truy cập ứng dụng demo và database riêng. Mọi thông báo nằm trong UniSupport. Việc chia module là chia trách nhiệm nghiệp vụ trong codebase, không là yêu cầu triển khai 10 service độc lập.

[Về danh mục PRD](../README.md)
