# Liên kết PRD, Master và Charter — v5.1

requirements.json là manifest của 4 module, 24 FR, 149 AC/TC chức năng và 18 kiểm tra hệ thống. Master dùng nguyên mã và tên FR; không thêm hoặc bỏ chức năng. Form đặc tả của từng FR được giữ nguyên.

## Liên kết task và kiểm thử

Mỗi FR có parent 0h và 9 task: PM.01; BE.01–03; FE.01–02; QA.01–03. QA.02 chạy đúng TC của FR. Công việc nền, tích hợp, E2E/NFR/MET và bàn giao là task dự án dùng chung, không là module thứ năm. Tổng 256 task chi tiết và 6 mốc đã nằm trong số task đó.

Nguồn task chuẩn là `plan.tasks` trong requirements.json: mã, tên, role, FR, giờ, input/output/DoD, predecessors và ngày. Task theo cùng package tuần tự; dependency liên package Finish-to-Start theo lịch đã lập. Khi thay ngày, PM phải review các task nối tiếp và mốc; bản Gantt gọn không tự reschedule dependency.

## Baseline giờ

Trần giờ người dùng chốt: **288h chính + 32h dự phòng = 320h**. Dự phòng chưa giao task, chưa sử dụng, không cộng vào dòng task hoặc FR cha. Chưa có gross/net/actual mới nên không tự tính lại chi phí hoặc lợi nhuận.

| Vai trò | Giờ chính | Dự phòng chưa dùng | Tổng ngân sách giờ |
| --- | ---: | ---: | ---: |
| PM | 53 | 8 | 61 |
| BE | 97.5 | 8 | 105.5 |
| FE | 64 | 8 | 72 |
| QA | 73.5 | 8 | 81.5 |
| **Tổng** | **288** | **32** | **320** |

Giữ nguyên giờ của 24 FR và phạm vi test. Điều chỉnh 34 task nền/dự án dùng chung để không tính lại phần đặc tả, triển khai và báo cáo đã phân cho từng FR. Các con số dưới đây là **phân bổ dự toán trong trần ngân sách để nhóm review**, không là giờ thực tế hoặc chứng minh đủ nguồn lực. Nếu review/triển khai cho thấy thiếu giờ, PM ghi chênh lệch và xét dự phòng/CR; không ép task thành Done để vừa 288h.

| Task | Giờ phân bổ | Căn cứ và giới hạn |
| --- | ---: | --- |
| PM-01 — Rà proposal và chốt phạm vi 4 module | 2 | Rà scope từ proposal/manifest đã có; không soạn lại yêu cầu của 24 FR. |
| PM-02 — Phân tích vai trò, trạng thái và quy tắc | 1.5 | Rà tập vai trò và trạng thái dùng chung; chi tiết quy tắc từng FR nằm trong PM.01 của FR. |
| PM-03 — Lập baseline ngày, effort và phụ thuộc | 1.5 | Chốt lịch ngày, giờ và dependency; bỏ mô hình capacity/what-if khỏi Excel theo yêu cầu mới. |
| PM-04 — Review thiết kế, contract và kế hoạch test | 2 | Review nền tảng và checklist dùng chung; review contract từng FR đã có giờ riêng. |
| PM-05 — Theo dõi 15 tuần và cập nhật quyết định | 15 | Giữ 15h cho 15 tuần theo dõi nhẹ; khoảng 1h/tuần, không coi là full-time mỗi ngày. |
| PM-06 — Chuẩn bị nội dung review giữa kỳ W7 | 1.5 | Tổng hợp đầu ra core và chuẩn bị review W7 từ evidence đã có. |
| PM-07 — Kiểm soát thay đổi và hoàn thiện phạm vi W8–11 | 2 | Theo dõi thay đổi trong baseline; phân tích lớn hoặc thêm scope phải có CR. |
| PM-08 — Rà hồ sơ nghiệm thu cuối W14 | 1.5 | Rà tính đủ của hồ sơ; không thay thực thi nghiệm thu/test. |
| PM-09 — Trình bày và bàn giao W15 | 2 | Chuẩn bị và trình bày/handoff từ tài liệu dev/QA đã có. |
| BE-01 — Thiết lập PHP/MySQL và cấu hình môi trường | 3 | Một cấu hình môi trường PHP/MySQL dùng chung cho prototype. |
| BE-02 — Thiết kế schema và hợp đồng API | 4 | Thiết kế schema/contract nền; query/projection từng FR có task riêng. |
| BE-03 — Tạo seed giả lập và script reset | 3 | Seed dùng chung và script reset; fixture từng FR có QA.01 riêng. |
| BE-04 — Tích hợp backend core W6 | 3 | Nối và rà build core; không tính lại lập trình/test từng endpoint. |
| BE-05 — Tích hợp backend đủ 4 module W11 | 3 | Nối và rà build đủ 4 module; không tính lại lập trình/test từng endpoint. |
| BE-06 — Xử lý phát hiện từ test phi chức năng | 3 | Giới hạn 3h xử lý phát hiện NFR; nếu vượt phải xét giờ dự phòng/CR. |
| BE-07 — Sửa lỗi backend trước regression cuối | 5 | Giữ 5h sửa lỗi backend trước regression; không đảm bảo số lỗi chưa biết. |
| BE-08 — Đóng gói source, DB và hướng dẫn chạy | 4 | Đóng gói source/DB và hướng dẫn triển khai prototype đã có. |
| FE-01 — Thiết kế wireframe desktop/mobile và nhãn | 3 | Wireframe và nhãn dùng chung; UI từng FR có FE.01 riêng. |
| FE-02 — Khởi tạo giao diện và client contract | 3 | Client/layout/error convention dùng chung; kết nối từng FR có FE.02 riêng. |
| FE-03 — Tích hợp giao diện core W6 | 2 | Rà routing/menu và build core; không làm lại UI từng FR. |
| FE-04 — Tích hợp giao diện đủ 4 module W11 | 2 | Rà routing/menu và build đủ 4 module; không làm lại UI từng FR. |
| FE-05 — Rà mobile, bàn phím và lỗi sử dụng | 2 | Rà desktop/mobile/keyboard theo mẫu màn hình dùng chung. |
| FE-06 — Sửa lỗi giao diện trước regression cuối | 3 | Giữ 3h sửa lỗi FE trước regression; vượt giờ xét dự phòng/CR. |
| FE-07 — Hoàn thiện hướng dẫn thao tác cho handoff | 1 | Tổng hợp hướng dẫn từ màn hình và luồng đã làm. |
| QA-01 — Rà AC và ma trận kiểm thử | 2.5 | Rà mapping AC/TC đã có trong PRD; không soạn lại đặc tả từng FR. |
| QA-02 — Soạn case, fixture và expected cho 24 FR | 6 | Rà và chuẩn hóa 149 TC đã soạn trong PRD cùng fixture nền; chưa thực thi test. |
| QA-03 — Kiểm tra build và bằng chứng giữa kỳ | 1.5 | Kiểm readiness/evidence build W7; thực thi TC core có task theo FR. |
| QA-04 — Chạy E2E-01..08 xuyên module | 4 | Giữ đủ 4h thực thi 8 E2E xuyên module, tách khỏi test từng FR. |
| QA-05 — Chạy 6 kiểm tra NFR | 3 | 3h cho 6 NFR prototype, dùng môi trường/fixture nền; chưa là kiểm định production. |
| QA-06 — Đo MET-01..04 mục tiêu proposal | 3 | 3h cho 4 phép đo MET theo kịch bản giả lập đã định nghĩa. |
| QA-07 — Retest và regression bản cuối | 5 | Giữ 5h retest/regression cuối; số bug thực tế có thể cần dự phòng. |
| QA-08 — Rà bộ nghiệm thu W14 | 1.5 | Rà hồ sơ nghiệm thu từ kết quả test/evidence, không chạy lại toàn bộ TC. |
| QA-09 — Hoàn thiện test report và hướng dẫn | 2.75 | Tổng hợp báo cáo/hướng dẫn từ báo cáo từng FR; không tạo evidence chưa có. |
| QA-10 — Kiểm tra người khác chạy lại khi handoff | 1.5 | Kiểm tra chạy lại theo hướng dẫn và seed/reset đã đóng gói. |

## Excel và ClickUp

Excel chỉ còn sheet **Master_Gantt**, không có cột trạng thái/Backlog, capacity, điều chuyển hoặc các sheet phụ. Giữ các khoảng ngày dự kiến và mốc của lịch cũ; effort giảm không tự rút ngắn lịch 15 tuần. Cột G tính giờ task; FR cha 0h. Thanh Gantt cập nhật theo ngày start/due và FR cha lấy MIN/MAX ngày task con, nhưng không tự dịch lịch task nối tiếp.

CSV nhập ClickUp dùng cùng 24 FR, 256 task, 167 case con, ngày và estimate. Case con/FR cha 0h để không tính giờ hai lần. Đổi ngày/giờ trong Excel phải cập nhật `plan.tasks` và xuất CSV lại; CSV đã tải không tự cập nhật. Không xuất CSV bằng sheet phụ vì sheet đó đã được bỏ.

Predecessors ghi trong `plan.tasks` và mô tả CSV; sau import phải nối native dependencies và kiểm mapping theo mã. ID trong mô tả không phải dependency native đã tạo. Assignee để trống đến khi có thành viên thật. `Status=to do` chỉ dùng để nhập workflow mở vào ClickUp, không là cột trạng thái Excel. QA Result vẫn Not Run đến khi thực thi; không điền Pass/evidence giả.
