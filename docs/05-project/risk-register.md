# Sổ rủi ro UniSupport

Đánh giá định tính đề xuất cho prototype. PM/BA rà ở buổi cuối tuần và trước mỗi mốc. Mỗi owner cập nhật dấu hiệu, trạng thái và issue xử lý khi phát sinh. Trạng thái dưới đây là rủi ro đang mở để theo dõi.

| Mã | Mô tả và dấu hiệu kích hoạt | Khả năng | Tác động | Owner | Xử lý | Mốc | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- | --- |
| R-01 | Quy trình hoặc quyền thay đổi. OQ-02/03 quá hạn hoặc xuất hiện yêu cầu ngoài Proposal. | Vừa | Cao | PM/BA | Ghi quyết định chờ, giữ giả định được nêu, mở CR nếu đổi hành vi hoặc phạm vi. | MS-01, MS-03, MS-05 | Mở |
| R-02 | Nguồn lực giảm. Giờ đã xác nhận thấp hơn phân bổ hoặc có task dự báo trễ. | Vừa | Cao | PM/BA | Lập lại phân bổ theo predecessor, báo mốc, xét dự phòng rồi trình duyệt nếu phải dời. | MS-01–MS-06 | Mở |
| R-03 | Ghi lịch sử và hồ sơ không nhất quán khi giao việc hoặc cập nhật song song. Một test rollback, gửi lặp hoặc cũ phiên bản bị Fail. | Vừa | Cao | BE | Kiểm transaction MySQL, ràng buộc duy nhất và test rollback/concurrency ở cổng phát sinh. | MS-02, MS-03, MS-04 | Mở |
| R-04 | Quyền đối tượng hoặc phạm vi báo cáo bị áp dụng sai. API cho actor khác nhận hồ sơ hoặc số tổng. | Vừa | Cao | BE | Rà ma trận quyền và kiểm trước truy vấn, chạy lại kiểm quyền tại từng mốc. | MS-01–MS-05 | Mở |
| R-05 | Kịch bản di động bị chặn hoặc số đo hiệu năng vượt ngưỡng trên máy thử. | Vừa | Vừa | FE | Kiểm viewport và bộ dữ liệu tải sớm theo NFR, ghi kết quả và retest trước mốc. | MS-02–MS-06 | Mở |
| R-06 | Phép đo MET thiếu mẫu hoặc dưới mục tiêu. | Vừa | Vừa | QA | Lưu số liệu trước/sau và mẫu, phân tích nguyên nhân, trình PM/BA và đại diện nghiệp vụ quyết định. | MS-06 | Mở |
| R-07 | Người tiếp nhận không cài lại được do thiếu phiên bản, cấu hình, dữ liệu hoặc hướng dẫn. | Vừa | Cao | QA | Yêu cầu người ngoài phần phát triển chạy E2E-08 trên môi trường sạch và ghi bước thiếu. | MS-06 | Mở |
| R-08 | Khối lượng kiểm thử mới vượt giờ của task QA hiện tại. | Cao | Cao | QA | Đo trên build đầu tiên, dự báo chênh lệch, ưu tiên lỗi quyền/ghi dữ liệu, xét dự phòng hoặc CR trước khi đổi ngày. | MS-01–MS-06 | Mở |
