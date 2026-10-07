# Chuyển trạng thái

| Từ | Sang | FR | Ai | Điều kiện |
| --- | --- | --- | --- | --- |
| Chưa tồn tại | Đã tiếp nhận | FR-STU-01 | Sinh viên | Đủ trường, phiên hợp lệ |
| Đã tiếp nhận | Đang xử lý | FR-DSP-06 | Người được giao | Có trách nhiệm hợp lệ |
| Đang xử lý | Chờ bổ sung | FR-DSP-07 | Người được giao | Câu hỏi không rỗng; chưa có câu hỏi mở |
| Chờ bổ sung | Đang xử lý | FR-STU-04 | Chủ sinh viên | Trả lời câu hỏi mở |
| Đang xử lý | Đã giải quyết | FR-DSP-09 | Người được giao | Có kết quả; không câu hỏi mở |
| Đã giải quyết | Đã đóng | FR-DSP-10 | Điều phối viên | Đúng phạm vi; có kết quả |

Mọi chuyển khác bị từ chối; dữ liệu và lịch sử không được ghi một phần. So sánh record_version trước khi ghi. Gửi lặp không tạo hai sự kiện hoặc thay thời điểm đầu tiên. Yêu cầu đã đóng chỉ đọc và nhận phản hồi, không nhận tiến độ mới.
