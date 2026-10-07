# Thuật ngữ nghiệp vụ

| Thuật ngữ | Ý nghĩa |
| --- | --- |
| Ticket | Một yêu cầu hỗ trợ có mã duy nhất, chủ sở hữu và vòng đời. |
| Hàng chờ | Ticket RECEIVED chưa phân công trong phòng ban; dùng để tiếp nhận. |
| Công việc của tôi | Ticket đang được phân công cho NV trong phiên; có thể lọc trạng thái. |
| Claim hoặc nhận | NV nhận Ticket chưa phân công và bắt đầu PROCESSING trong cùng thao tác. |
| Assign hoặc phân công | QL giao Ticket RECEIVED chưa phân công cho một NV; chưa bắt đầu xử lý. |
| Reassign hoặc đổi người | QL đổi NV trong cùng phòng ban và giữ diễn biến nghiệp vụ. |
| Transfer hoặc chuyển phòng ban | QL đổi phòng ban và loại, bỏ người cũ, đưa về RECEIVED. |
| Information request | Một yêu cầu bổ sung OPEN hoặc ANSWERED; một câu trả lời kết thúc mỗi vòng. |
| Outcome | Kết quả RESOLVED hoặc REJECTED được giữ sau CLOSED. |
| SLA | Số giờ liên tục từ created_at đến due_at; không là cam kết vận hành thật. |
| Quá hạn đang mở | Ticket trong ba trạng thái mở có due_at nhỏ hơn thời điểm đang xét. |
| Idempotency | Gửi lại cùng một thao tác cho cùng kết quả, không thêm bản nghiệp vụ. |
| Version | Số phiên bản Ticket chống cập nhật bằng dữ liệu đã cũ. |
| Domain event | Sự kiện nghiệp vụ đã commit dùng cho lịch sử và xử lý thông báo. |
| AC | Tiêu chí nghiệm thu có mã riêng, xác định kết quả bắt buộc. |
| TS | Kịch bản kiểm thử; QA triển khai thành test case chi tiết và lưu bằng chứng. |

[Về danh mục PRD](../README.md)
