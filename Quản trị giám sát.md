# Quản trị và giám sát UniSupport

## Nguồn theo dõi

GitHub Issues và Milestones theo dõi công việc, lỗi, phản hồi, quyết định và thay đổi. [Manifest](docs/05-project/requirements.json) giữ baseline mã, ngày, giờ, phụ thuộc và mốc. [Master Plan Excel](UniSupport_Master_Plan.xlsx) trình bày lịch và nguồn lực. [Sổ rủi ro](docs/05-project/risk-register.md) được cập nhật trong repo. Giờ thực tế và ngày dự báo lấy từ cập nhật của owner trong issue.

## Trách nhiệm và nhịp phối hợp

PM/BA tổng hợp, điều phối và ghi quyết định. Backend phụ trách API, dữ liệu và quyền. Frontend phụ trách giao diện và tích hợp. QA/tài liệu tổng hợp kết quả, lỗi và bằng chứng. Đại diện được ủy quyền của Phòng Công tác Sinh viên Aurora University (Mô phỏng) quyết định thay đổi cam kết và nghiệm thu. Sử dụng vai trò, nhịp họp và phản hồi trong [Team Charter](TeamCharter.md).

| Khi nào | Người gửi và người nhận | Thông tin |
| --- | --- | --- |
| Đầu tuần, đề xuất 20 phút | Owner → PM/BA và các vai trò phụ thuộc | Ưu tiên, task sẵn sàng, giờ dự kiến và blocker. |
| Cuối tuần, đề xuất 20 phút | PM/BA và QA → nhóm | Đầu ra so baseline, dự báo mốc, lỗi, rủi ro và người xử lý. Ghi vào issue liên quan. |
| Khi đến mốc | PM/BA → đại diện Aurora University (Mô phỏng) | Build, phạm vi giao, tiêu chí, bằng chứng QC và đề nghị quyết định. |
| Khi phát hiện chặn | Owner → PM/BA, vai trò có thể gỡ chặn | Link issue, việc chặn, mốc bị ảnh hưởng và dự báo. PM/BA phản hồi trong phiên làm việc kế tiếp. |

## Chỉ số và chuyển cấp

Tại ngày chốt báo cáo, lấy các task công việc trong phạm vi (không cộng issue cha FR, mốc hoặc dự phòng). PV là giờ baseline dự kiến đến ngày chốt theo phân bổ ngày. EV là tổng giờ baseline của task có bằng chứng hoàn thành hợp lệ đến ngày chốt. Task 6 giờ có trọng số gấp 12 lần task 0,5 giờ. AC là tổng giờ thực tế đã ghi cho cùng phạm vi và cửa sổ. SPI = EV/PV khi PV > 0. CPI = EV/AC khi AC > 0 và dữ liệu giờ thực tế đã đầy đủ. Ghi N/A khi thiếu mẫu hoặc mẫu số bằng 0. Issue đóng thiếu bằng chứng cần xác minh trước khi đưa vào EV.

Báo vàng khi SPI dưới 1 hoặc dự báo xuất hiện chênh lệch effort cần phương án sửa. Báo đỏ khi dự báo vượt ngày mốc hoặc cổng còn lỗi nghiêm trọng. Owner tạo/cập nhật issue ngay khi phát hiện, PM/BA rà thứ tự và dự phòng, chuyển đại diện khách hàng nếu ảnh hưởng cam kết. Theo [QC](qc%26qa/QC.md#51-phân-loại-và-ưu-tiên), mức lỗi là **Nghiêm trọng** hoặc **Thông thường**. Ưu tiên theo tác động và mốc liên quan. Thời hạn xử lý được ghi theo từng lỗi, sau khi có đánh giá.

## Thay đổi và hồ sơ quyết định

Dùng [quy trình kiểm soát thay đổi](docs/05-project/change-control.md) làm nguồn duy nhất cho phê duyệt và đồng bộ. Biên bản trên issue ghi ngày, người quyết định, lựa chọn, lý do, người theo dõi và liên kết PR/bằng chứng.
