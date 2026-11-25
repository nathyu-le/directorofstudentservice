# Câu hỏi cần xác nhận trước khi cam kết

Hạn là mốc ra quyết định **dự kiến** theo lịch mô phỏng, kể cả ngày đã qua. Khi chưa có bằng chứng phản hồi, giữ trạng thái Mở và chuyển PM/BA rà tác động tại buổi gần nhất. Khi chốt, ghi ngày, người trả lời và issue quyết định.

| Mã | Cần chốt | Người trả lời (vai trò) | Hạn dự kiến | Ảnh hưởng | Khi chưa có trả lời | Trạng thái |
| --- | --- | --- | --- | --- | --- | --- |
| OQ-01 | Ngày khởi động, ngày nghỉ, lịch dùng thử | Đầu mối Phòng Công tác Sinh viên và PM/BA | 12/10/2026 | MS-01–06. Dời ngày có thể đổi cả task nối tiếp và thanh toán. | Dùng lịch minh họa, chưa cam kết. R-02. | Mở |
| OQ-02 | Năm trạng thái, ai đóng, có hủy/mở lại không | Đại diện nghiệp vụ điều phối | 30/10/2026 | MS-02–05. STU-02/03/04, DSP-03–10, RPT-01/02/05. Rà thêm giờ API/UI/QA nếu thêm trạng thái. | Giữ năm trạng thái đề xuất. R-01. | Mở |
| OQ-03 | Danh mục và phạm vi điều phối/báo cáo | Đầu mối nghiệp vụ và người quản trị quyền của khách | 30/10/2026 | MS-01–05. IAM-03, DSP-01–05, RPT-01–06. Có thể đổi seed và effort kiểm quyền. | Dùng PB-A/B và danh mục giả lập. R-04. | Mở |
| OQ-04 | Trường dữ liệu, độ dài, phiên và giới hạn đăng nhập | Đầu mối nghiệp vụ, BE/FE/QA rà khả năng thực hiện | 30/10/2026 | MS-01–04. IAM-01, STU-01/02/04/05, DSP-03/04/05/07/08/09. Rà validation và fixture. | Giữ mặc định đề xuất của PRD. R-01. | Mở |
| OQ-05 | Người được bố trí, giờ làm và lịch nghỉ | Ban quản lý dự án và người điều phối nguồn lực | 12/10/2026 | MS-01–06. 4 giờ/vai trò/ngày là giả định. Có thể đổi ngày và phân bổ. | Dùng năng lực mô phỏng để xếp lịch, chưa cho mượn nguồn lực. R-02. | Mở |
| OQ-06 | Hạn xử lý và cách tính kỳ báo cáo/phản hồi | Đại diện điều phối và quản lý báo cáo | 27/11/2026 | MS-03–05. DSP-05 và RPT-01–06. Ước lượng lại query và kiểm biên nếu đổi. | Giữ hạn nhập tay, backlog hiện hành và kỳ cập nhật phản hồi. R-01. | Mở |
| OQ-07 | Nhãn Việt/Anh, viewport, cấu hình máy thử | Đại diện nghiệp vụ và QA | 05/11/2026 | MS-01–06. NFR-USAB-01, PERF-01, DEP-01. Có thể đổi giờ chạy test/retest. | Ghi như mục tiêu prototype và công bố điều kiện đo. R-05, R-07. | Mở |
| OQ-08 | Phân bổ 288 giờ và cách dùng 32 giờ dự phòng | PM/BA, BE, FE, QA, đại diện khách nếu đổi cam kết | 06/11/2026 | MS-01–06, Q-01–06. Rà giờ mới khi có build hoặc phát hiện blocker. | Giữ trần dự toán đề xuất, chưa coi là giờ thực tế. R-02, R-08. | Mở |
