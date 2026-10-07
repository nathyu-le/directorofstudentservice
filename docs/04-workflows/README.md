# Danh mục quy trình nghiệp vụ

| Mã | Tên | Kết quả |
| --- | --- | --- |
| WF-01 | Sinh viên gửi yêu cầu hỗ trợ | Một Ticket RECEIVED, mã duy nhất, hạn hợp lệ, chưa phân công; SV nhận mã và QL thấy hàng chờ. |
| WF-02 | Tiếp nhận và bắt đầu xử lý | Ticket PROCESSING, một người phụ trách, started_at lần đầu và lịch sử đúng nhánh. |
| WF-03 | Yêu cầu và trả lời bổ sung thông tin | Cùng mã Ticket, một câu trả lời cho vòng hỏi; Ticket trở về PROCESSING; hạn không bị dừng hoặc đặt lại. |
| WF-04 | Chuyển phòng ban và tiếp nhận lại | Giữ mã, chủ, nội dung gốc, lịch sử và hạn; trách nhiệm mới được xác lập tại đích. |
| WF-05 | Ghi kết quả giải quyết hoặc từ chối | Có đúng một kết quả xử lý, ended_at hợp lệ; Ticket chỉ đọc đối với nghiệp vụ xử lý. |
| WF-06 | Sinh viên đóng Ticket và đánh giá | Ticket CLOSED, có hoặc chưa có feedback; kết quả xử lý cũ được giữ. |


Workflow phối hợp nhiều FR theo một tình huống đầu cuối; không là một mã chức năng mới. Hai nhánh trong cùng workflow được kiểm thử riêng. Bảng chuyển trạng thái là nguồn quyết định trạng thái; quyền và BR vẫn áp dụng tại từng bước. Mỗi workflow có mã E2E trong phần nghiệm thu, cùng dữ liệu và kết quả dự kiến.

[Về danh mục PRD](../README.md)
