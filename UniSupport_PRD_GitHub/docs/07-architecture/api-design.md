# Hợp đồng điểm truy cập đề xuất

Đường dẫn minh họa để liên kết FR và test; nhóm có thể chọn route PHP cụ thể nhưng phải cập nhật mapping cùng PRD. Không yêu cầu framework/API platform ngoài proposal.

| FR | Method | Route gợi ý | Quyền | Kết quả |
| --- | --- | --- | --- | --- |
| FR-STU-01 | POST | /prototype/stu/fr-stu-01 | Sinh viên | Tạo yêu cầu có mã để trường tiếp nhận và sinh viên theo dõi. |
| FR-STU-02 | GET | /prototype/stu/fr-stu-02 | Sinh viên | Tìm yêu cầu mình đã gửi và biết trạng thái hiện tại. |
| FR-STU-03 | GET | /prototype/stu/fr-stu-03 | Sinh viên | Biết yêu cầu đã được tiếp nhận, ai chịu trách nhiệm và có cần bổ sung thông tin. |
| FR-STU-04 | POST | /prototype/stu/fr-stu-04 | Sinh viên | Trả lời yêu cầu bổ sung trên cùng hồ sơ hỗ trợ. |
| FR-STU-05 | POST | /prototype/stu/fr-stu-05 | Sinh viên | Ghi nhận phản hồi và điểm hài lòng về kết quả. |
| FR-DSP-01 | GET | /prototype/dsp/fr-dsp-01 | Điều phối viên | Nhận biết yêu cầu cần phân loại hoặc phân công. |
| FR-DSP-02 | GET | /prototype/dsp/fr-dsp-02 | Điều phối viên / Nhân viên xử lý | Đọc thông tin cần xử lý và truy vết thao tác theo trách nhiệm. |
| FR-DSP-03 | POST | /prototype/dsp/fr-dsp-03 | Điều phối viên | Gán loại vấn đề để định tuyến và tổng hợp báo cáo. |
| FR-DSP-04 | POST | /prototype/dsp/fr-dsp-04 | Điều phối viên | Đặt một đơn vị và một người chịu trách nhiệm hiện tại, kể cả khi phối hợp đổi đơn vị. |
| FR-DSP-05 | POST | /prototype/dsp/fr-dsp-05 | Điều phối viên | Cung cấp mốc theo dõi quá hạn cho yêu cầu; không tự áp SLA thực tế. |
| FR-DSP-06 | POST | /prototype/dsp/fr-dsp-06 | Nhân viên được giao | Xác nhận bắt đầu xử lý yêu cầu đã tiếp nhận. |
| FR-DSP-07 | POST | /prototype/dsp/fr-dsp-07 | Nhân viên được giao | Nêu thông tin còn thiếu và đặt yêu cầu vào trạng thái chờ trả lời. |
| FR-DSP-08 | POST | /prototype/dsp/fr-dsp-08 | Nhân viên được giao | Ghi công việc đã thực hiện để phối hợp và theo dõi. |
| FR-DSP-09 | POST | /prototype/dsp/fr-dsp-09 | Nhân viên được giao | Ghi kết quả để sinh viên nhận và phản hồi. |
| FR-DSP-10 | POST | /prototype/dsp/fr-dsp-10 | Điều phối viên | Kết thúc hồ sơ đã có kết quả sau kiểm tra; bảo toàn lịch sử. |
| FR-RPT-01 | GET | /prototype/rpt/fr-rpt-01 | Quản lý | Đếm yêu cầu theo trạng thái để theo dõi tổng thể. |
| FR-RPT-02 | GET | /prototype/rpt/fr-rpt-02 | Quản lý | Nhận biết yêu cầu còn mở đã vượt hạn dự kiến. |
| FR-RPT-03 | GET | /prototype/rpt/fr-rpt-03 | Quản lý | Đo thời gian từ gửi đến giải quyết bằng dữ liệu mẫu. |
| FR-RPT-04 | GET | /prototype/rpt/fr-rpt-04 | Quản lý | Biết nhóm vấn đề nào có nhiều yêu cầu. |
| FR-RPT-05 | GET | /prototype/rpt/fr-rpt-05 | Quản lý | Biết trách nhiệm hiện hành và lượng việc còn mở của các đơn vị. |
| FR-RPT-06 | GET | /prototype/rpt/fr-rpt-06 | Quản lý | Tổng hợp phản hồi kết quả để đánh giá chất lượng hỗ trợ. |
| FR-IAM-01 | POST | /prototype/iam/fr-iam-01 | Sinh viên / Điều phối viên / Nhân viên / Quản lý | Xác thực tài khoản mẫu trước khi sử dụng chức năng. |
| FR-IAM-02 | POST | /prototype/iam/fr-iam-02 | Mọi vai trò đã đăng nhập | Kết thúc phiên sử dụng hiện tại. |
| FR-IAM-03 | POST | /prototype/iam/fr-iam-03 | Máy chủ cho mọi vai trò | Chỉ cho truy cập thông tin và thao tác trong trách nhiệm được cấp. |

Quy ước: đọc không thay dữ liệu; thao tác ghi kiểm tra CSRF, đầu vào, quyền, trạng thái và version. Trả 401 khi chưa xác thực, 403/404 phù hợp để tránh lộ đối tượng, 422 lỗi trường, 409 stale version/trạng thái; lỗi máy chủ không trả SQL/secret. Response thành công nêu mã đối tượng và trạng thái hiện hành. Hợp đồng mã lỗi là đề xuất, cần nhất quán giữa BE/FE và TC.
