# Miền nghiệp vụ hỗ trợ sinh viên

Ticket là đơn vị nghiệp vụ xuyên suốt từ gửi yêu cầu đến đóng và đánh giá. Loại vấn đề thuộc một phòng ban, giúp xác định nơi tiếp nhận và SLA tại thời điểm tạo. Phòng ban và người phụ trách hiện tại cho biết trách nhiệm hiện hành; lịch sử giữ trách nhiệm trước đó. Ticket không được tạo lại khi đổi người, chuyển phòng ban hoặc bổ sung thông tin.

Trạng thái diễn tả Ticket đang ở bước nào; assignee_id diễn tả ai chịu trách nhiệm; is_overdue diễn tả hạn hiện tại có bị vượt hay không. Ba thông tin độc lập: một Ticket RECEIVED có thể đã phân công nhưng chưa bắt đầu, hoặc chưa phân công và đã quá hạn. Giao diện và báo cáo phải giữ sự phân biệt này.

Khi nhân viên hoàn tất hoặc từ chối, Ticket đã có kết quả nhưng chưa chắc sinh viên xác nhận đóng. outcome và ended_at ghi kết quả xử lý, còn closed_at ghi việc sinh viên xác nhận đóng. Tách hai mốc giúp báo cáo thời gian giải quyết không tăng vì sinh viên chậm đóng, và giúp CLOSED vẫn phân biệt kết quả thành công với từ chối.

[Về danh mục PRD](../README.md)
