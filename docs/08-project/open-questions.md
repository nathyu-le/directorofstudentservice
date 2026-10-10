# Câu hỏi cần xác nhận trước baseline

| ID | Cần chốt | Nguồn hiện có | Người lấy xác nhận | Trạng thái |
| --- | --- | --- | --- | --- |
| OQ-01 | Ngày bắt đầu15tuần, ngày nghỉ/lịch trường | 12/10/2026 là minh họa, không ngày đã duyệt | PM | Open |
| OQ-02 | 5trạng thái, ai đóng, cóhủy/mở lại không | v5 đềxuấtkhônghủy/mở lại | PM+stakeholder | Open |
| OQ-03 | Danh mục phòng/loại, quyền dispatch/report | Seed PB-A/B, không danh mục thật | PM+BE | Open |
| OQ-04 | Trường đầu vào, độdài, cfgsession/ratelimit | v5 đề xuất rõ default | BE/FE/QA+PM | Open |
| OQ-05 | Danh tính4người, availability, externalwork, leave, reserve ngày | Chưa có, lịch4h/ngày chỉ minh họa | PM+các role | Open |
| OQ-06 | Hạn xử lý/vùngreport/định nghĩa kỳ phản hồi | Due nhập tay, backlog current, feedback updated_at | PM+reviewer | Open |
| OQ-07 | Yêu cầu Việt/Anh, viewport và môi trường NFR | Proposal yêu cầu cân nhắc thiết kế | PM+FE/QA | Open |
| OQ-08 | Estimate chi tiết/288h chính+32h dự phòng | Baseline nguồn lực đề xuất, không actual/proposalcosthours | PM+4role | Open |

Thiếu dữ liệu nguồn không bịa để đóng câu hỏi. Nhóm có thể review/defaultprototype nhưng phải lưu đầy đủ quyết định và người xác nhận. Date, effort và capacity Master chỉ dự kiến đến khi OQ-01/05/08chốt.

## Review giờ v5.1

Trần 288h chính/32h dự phòng đã được người dùng chốt. Phân bổ giờ theo role/task vẫn là đề xuất. BE/FE/QA cần review theo build/độ phức tạp; chênh lệch ghi rõ và trình PM xử lý dự phòng/CR. Gantt một sheet không đo hoặc chứng minh availability thật.
