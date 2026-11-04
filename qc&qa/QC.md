# UniSupport — Kế hoạch kiểm soát chất lượng QC

**Phiên bản:** 2.0 · **Ngày soạn:** 04/11/2026 · **Trạng thái:** Kế hoạch thực hiện, chờ review; chưa có kết quả kiểm thử xác nhận trong repo.

## 1. Mục tiêu và phạm vi kiểm thử

QC (Quality Control) kiểm tra sản phẩm trên một build cụ thể, so actual với expected, cung cấp bằng chứng, theo dõi sửa lỗi và xác minh lại. [QA.md](QA.md) thiết lập tiêu chuẩn và giám sát/cải tiến quy trình; file này mô tả việc thực thi kiểm thử, kiểm soát lỗi và đề nghị nghiệm thu.

Căn cứ là PRD/Master Plan v6.0 tại commit `dfa151a619b914aa77a29fdf05b5675af4e42cf0`. Scope gồm 24 FR, 149 TC chức năng và 18 kiểm tra hệ thống. Unit/self-check và các lần chạy lại không được cộng thành chức năng hoặc làm tăng giả số case baseline.

| Module | FR | TC chức năng |
| --- | --- | --- |
| STU — Cổng hỗ trợ sinh viên | 5 | 32 |
| DSP — Điều phối và xử lý yêu cầu | 10 | 64 |
| RPT — Quản lý và báo cáo | 6 | 37 |
| IAM — Tài khoản và quyền truy cập | 3 | 16 |

**Trong phạm vi:** hành vi nghiệp vụ và quyền của STU/DSP/RPT/IAM; tính nhất quán dữ liệu, retry/concurrency; tích hợp UI–API–DB; E2E; sáu NFR và bốn MET có trong nguồn; kiểm khả năng người khác dựng lại prototype.

**Ngoài phạm vi:** production toàn trường, dữ liệu sinh viên thật, tích hợp thật với mọi hệ thống, tạo tài khoản/CRUD admin, reset mật khẩu, upload, email/SMS, xuất dữ liệu, KB/FAQ, thanh toán hoặc module mới. Không biến ví dụ đăng ký/thanh toán hoặc yêu cầu 500 ms/1.000 người dùng trong bài mẫu thành test UniSupport.

**Trạng thái quan sát ngày 04/11/2026:** các TC nguồn và system checks đều `Not Run`; repo được đọc chưa có application source, test automation hoặc CI configuration. Không có cơ sở ghi Pass hay “đã chạy”. Nếu nhóm chỉ cung cấp UI prototype, QC chỉ kết luận được điều hướng/trạng thái/hiển thị trong bản UI đã kiểm; quyền server, transaction, lưu dữ liệu và số liệu báo cáo thật cần build tích hợp. Case chưa chạy giữ Not Run; khi xác định thiếu đầu vào ngăn chạy thì ghi Blocked với lý do, không đổi toàn bộ trạng thái nguồn một cách mặc định.

Các quy ước mới về quản lý lượt chạy, severity và mẫu báo cáo trong tài liệu này là đề xuất tổ chức thực hiện, không phải thông tin cho rằng nhóm đã áp dụng. Thiết kế đang gắn OQ cần review; test đạt trên thiết kế đề xuất không đồng nghĩa stakeholder đã duyệt rule đó.

## 2. Trách nhiệm và đầu vào bàn giao

| Vai trò hiện có | Trách nhiệm thực thi QC | Đầu ra |
| --- | --- | --- |
| BE | Unit/component và self-check BE; cung cấp API/schema/seed/build; hỗ trợ đối chiếu dữ liệu; sửa lỗi BE | Kết quả self-check, PR/commit, build và hướng dẫn test, bản sửa liên kết defect |
| FE | Unit/component hoặc self-check FE phù hợp mã có thật; kiểm contract/tích hợp, trạng thái UI, mobile; sửa lỗi FE | Bản UI/tích hợp, PR, cấu hình/browser/viewport và self-check |
| QA/tài liệu khi làm QC | Chuẩn bị/kiểm fixture; thực thi các test được giao, ghi actual/evidence; tạo defect, retest/regression và test report | Run record, defect và kết quả xác minh; không tự tạo Pass |
| PM/BA và người có thẩm quyền | Làm rõ expected/OQ; thống nhất mức ưu tiên; review kết quả nghiệp vụ và quyết định chấp nhận | Quyết định có người/ngày thật và vấn đề còn mở |

Dev chịu trách nhiệm kiểm thử đơn vị, QC xem bằng chứng và kiểm độc lập ở cấp phù hợp; không giao toàn bộ unit test cho QC rồi bỏ self-check. UAT do người đại diện nghiệp vụ/được thầy hoặc nhóm có thẩm quyền chỉ định đánh giá, QC hỗ trợ. Chưa có người được chỉ định thì ghi chờ xác nhận, không tự đặt tên Product Owner/khách hàng giả.

## 3. Chiến lược theo rủi ro và cấp kiểm thử

### 3.1. Các rủi ro cần ưu tiên

| Rủi ro UniSupport | Phạm vi và điều cần quan sát | Oracle hoặc bằng chứng |
| --- | --- | --- |
| Lộ hồ sơ/ghi chú hoặc nâng quyền | IAM-03; quyền Student/handler/dispatch/report; hồ sơ ngoài scope; NV cũ sau đổi người phụ trách | Gọi API trực tiếp theo actor; response không lộ nội dung; dữ liệu không đổi khi từ chối; E2E-06/NFR-SEC-01 |
| Gửi trùng hoặc cập nhật đè | STU-01, các mutation, request cũ và hai thao tác đồng thời | Số request/child/history, record_version, phản hồi replay/conflict; E2E-07/NFR-REL-01 |
| Sai vòng đời | Bắt đầu, yêu cầu/trả lời bổ sung, giải quyết và đóng | Trạng thái và timestamp trước/sau, điều kiện question/result, lịch sử; E2E-01/02 |
| Sai trách nhiệm hoặc hạn | DSP-04/05, quyền sau chuyển PB/người, trước/đúng/sau hạn | Assignee/scope hiện hành, audit history, as_of/due_at; E2E-03/04 |
| Báo cáo sai nhưng UI trông hợp lý | RPT-01…06; scope, filter, kỳ, bản ghi rỗng, nhiều history/feedback | Fixture có đáp án, query/tập nguồn độc lập, sample_count/công thức; E2E-05 |
| Không dùng/chạy lại được | Luồng sinh viên trên desktop/mobile và gói source/seed/guide | NFR-USAB-01; người khác chạy môi trường sạch với E2E-08/NFR-DEP-01 |

Ưu tiên chạy sớm phần quyền và toàn vẹn dữ liệu khi build tương ứng sẵn; trước retest xác định phạm vi ảnh hưởng. Ưu tiên không có nghĩa bỏ những case còn lại: kết luận FR vẫn phải đáp ứng toàn bộ AC/TC hiện hành.

### 3.2. Cấp kiểm thử và phương pháp

| Cấp | Mục tiêu áp dụng | Thực hiện và điều kiện chuyển tiếp |
| --- | --- | --- |
| Unit/component | Hàm/thành phần validation, rule quyền/trạng thái, xử lý gửi lặp, công thức báo cáo hoặc UI component trong code thực tế | Dev viết/chạy hoặc self-check có thể tái hiện; cần code và expected. Gắn kiểm soát vào `.BE.03`/self-check FE; chưa có danh mục unit tự động để tuyên bố coverage |
| Integration | UI–API–DB, middleware, ghi nghiệp vụ cùng history; lỗi truyền dữ liệu, rollback và xung đột | FE/BE self-check trước `.QA.02`; QC kiểm trên build thật, đối chiếu response và dữ liệu. Mock API chỉ giúp thử FE, không chứng minh tích hợp thật |
| System | Hành vi toàn ứng dụng theo 149 TC, E2E và NFR; đo các MET đã định nghĩa | QC thực thi đúng case/fixture trên build có đủ luồng; phân biệt case baseline với run và test hồi quy |
| Acceptance/UAT | Đại diện nghiệp vụ xác nhận quy trình đáp ứng nhu cầu trong phạm vi prototype | Dùng luồng đã có ở E2E-01/02/05/08 làm kịch bản demo/nghiệm thu; người nghiệp vụ đánh giá kết quả và ghi quyết định riêng; không gọi kết quả QC kỹ thuật là UAT đã ký |

Functional/non-functional là **loại kiểm thử**; manual/automation là **cách thực hiện**, không thay test level. Automation được xem ở mục 10. Không áp đầy đủ một quy trình production lên prototype khi chưa có căn cứ.

## 4. Môi trường, dữ liệu và điều kiện bắt đầu

### 4.1. Thông tin phải ghi cho mỗi build/lượt chạy

- Commit/build ID và phạm vi chức năng được bàn giao; đường dẫn truy cập hoặc cách khởi chạy thực tế.
- OS, browser và version, viewport, PHP/MySQL version, cấu hình ảnh hưởng phép đo. Nhóm cần xác nhận version; tài liệu không tự điền cấu hình chưa biết.
- Schema/seed/fixture revision, cách reset, timezone và mốc `as_of` nếu kiểm báo cáo/hạn.
- Actor/role/capability và scope được cấp bằng seed; thông tin đăng nhập mẫu chỉ dùng dữ liệu tổng hợp, không đưa mật khẩu thật/token vào evidence.
- API/log/DB read access tối thiểu cho phép kiểm expected; cách thu và làm sạch bằng chứng.

### 4.2. Bộ dữ liệu cần chuẩn bị

| Tập fixture | Cách dùng và expected cần biết trước |
| --- | --- |
| Người dùng và scope | Sinh viên A/B, nhân viên xử lý A/B, capability dispatch/report và tài khoản inactive; vai trò không tự có capability ngoài seed |
| Yêu cầu và vòng đời | Hồ sơ ở các state thiết kế, có/không assignee, question mở/đã trả lời, result và feedback; liên kết chủ và scope xác định |
| Validation và concurrency | Dữ liệu rỗng/khoảng trắng/biên theo FR; submission_key giống/khác; version mới/cũ; điều kiện hai thao tác cùng dữ liệu |
| Báo cáo | Tập nguồn có số lượng/công thức tính trước, nhiều history/feedback để bắt join trùng, không có mẫu, biên ngày và trước/đúng/sau due |
| Đo NFR/MET | 1.000 yêu cầu tổng hợp cho NFR-PERF-01; tập đo M/N, K/N, A/B hoặc intent/request/queue theo từng MET |

Các tên A/B là nhãn fixture, không phải danh tính hoặc danh mục đơn vị thật. Lưu bảng đáp án và truy vấn/tập nguồn dùng tính độc lập trước khi chạy báo cáo; không lấy chính số UI đang hiện làm expected. Reset theo tiền điều kiện của từng case; không mặc định dữ liệu sót từ case trước. Dùng UTC phía server và hiển thị Asia/Ho_Chi_Minh theo thiết kế; ghi rõ mốc thời gian khi thử biên.

### 4.3. Entry criteria

1. FR/AC/TC và contract cùng revision; không còn câu hỏi chặn expected của case cần chạy.
2. Build, môi trường, quyền truy cập, schema/seed và actor đủ thông tin.
3. `.QA.01` hoàn tất fixture; build tích hợp/self-check liên quan đã sẵn theo predecessor của `.QA.02`.
4. Expected và cách thu bằng chứng xác định; case cần API/DB có đầu vào tương ứng.

Thiếu điều kiện thì ghi case/task bị chặn, lý do, owner gỡ và dependency; các case độc lập vẫn được chạy. Không ghi Not Run thành Fail do chưa có build, và không ghi Pass từ mock.

## 5. Danh mục 24 FR và truy vết kiểm thử

Mỗi hàng dẫn tới đặc tả gốc. Dải TC liên tiếp được lấy từ traceability hiện hành; toàn bộ tiền điều kiện, dữ liệu, bước và expected của từng TC nằm tại [test-cases.md](../docs/04-acceptance/test-cases.md). Tài liệu này không sao chép một bộ test thứ hai dễ lệch nguồn. QA/QC trong mã task là mã vai trò của Master; giữ nguyên dù tên tài liệu đã tách.

| FR và hành vi | Bộ TC | Mốc hoàn tất | Task thực thi và lịch dự kiến |
| --- | --- | --- | --- |
| [FR-STU-01](../docs/03-modules/01-student-support/requirements/FR-STU-01.md) Gửi yêu cầu hỗ trợ | 8 — TC-STU-01-01…TC-STU-01-08 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) | [FR-STU-01.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/65)<br>09/11/2026 |
| [FR-STU-02](../docs/03-modules/01-student-support/requirements/FR-STU-02.md) Xem danh sách yêu cầu của tôi | 5 — TC-STU-02-01…TC-STU-02-05 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) | [FR-STU-02.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/74)<br>10/11/2026 |
| [FR-STU-03](../docs/03-modules/01-student-support/requirements/FR-STU-03.md) Xem chi tiết và tiến độ yêu cầu | 5 — TC-STU-03-01…TC-STU-03-05 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) | [FR-STU-03.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/84)<br>12/11/2026 |
| [FR-STU-04](../docs/03-modules/01-student-support/requirements/FR-STU-04.md) Bổ sung thông tin được yêu cầu | 7 — TC-STU-04-01…TC-STU-04-07 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-STU-04.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/92)<br>04/12/2026 |
| [FR-STU-05](../docs/03-modules/01-student-support/requirements/FR-STU-05.md) Phản hồi kết quả hỗ trợ | 7 — TC-STU-05-01…TC-STU-05-07 | [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) | [FR-STU-05.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/102)<br>07/12/2026 |
| [FR-DSP-01](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-01.md) Xem hàng chờ điều phối | 4 — TC-DSP-01-01…TC-DSP-01-04 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) | [FR-DSP-01.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/109)<br>10/11/2026 |
| [FR-DSP-02](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-02.md) Xem hồ sơ xử lý và lịch sử nghiệp vụ | 4 — TC-DSP-02-01…TC-DSP-02-04 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) | [FR-DSP-02.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/120)<br>11/11/2026 |
| [FR-DSP-03](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-03.md) Phân loại yêu cầu | 8 — TC-DSP-03-01…TC-DSP-03-08 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-DSP-03.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/131)<br>12/11/2026 |
| [FR-DSP-04](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-04.md) Phân công trách nhiệm xử lý | 8 — TC-DSP-04-01…TC-DSP-04-08 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-DSP-04.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/138)<br>16/11/2026 |
| [FR-DSP-05](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-05.md) Đặt hạn xử lý dự kiến | 8 — TC-DSP-05-01…TC-DSP-05-08 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-DSP-05.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/147)<br>02/12/2026 |
| [FR-DSP-06](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-06.md) Bắt đầu xử lý yêu cầu | 6 — TC-DSP-06-01…TC-DSP-06-06 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-DSP-06.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/156)<br>17/11/2026 |
| [FR-DSP-07](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-07.md) Yêu cầu sinh viên bổ sung thông tin | 7 — TC-DSP-07-01…TC-DSP-07-07 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-DSP-07.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/168)<br>03/12/2026 |
| [FR-DSP-08](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-08.md) Ghi cập nhật tiến độ xử lý | 6 — TC-DSP-08-01…TC-DSP-08-06 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) | [FR-DSP-08.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/181)<br>18/11/2026 |
| [FR-DSP-09](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-09.md) Ghi kết quả giải quyết | 7 — TC-DSP-09-01…TC-DSP-09-07 | [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) | [FR-DSP-09.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/190)<br>19/11/2026 |
| [FR-DSP-10](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-10.md) Đóng yêu cầu đã giải quyết | 6 — TC-DSP-10-01…TC-DSP-10-06 | [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) | [FR-DSP-10.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/199)<br>08/12/2026 |
| [FR-RPT-01](../docs/03-modules/03-management-reporting/requirements/FR-RPT-01.md) Tổng hợp trạng thái yêu cầu | 6 — TC-RPT-01-01…TC-RPT-01-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) | [FR-RPT-01.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/208)<br>11/12/2026 |
| [FR-RPT-02](../docs/03-modules/03-management-reporting/requirements/FR-RPT-02.md) Theo dõi yêu cầu quá hạn | 6 — TC-RPT-02-01…TC-RPT-02-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) | [FR-RPT-02.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/218)<br>14/12/2026 |
| [FR-RPT-03](../docs/03-modules/03-management-reporting/requirements/FR-RPT-03.md) Thống kê thời gian xử lý | 6 — TC-RPT-03-01…TC-RPT-03-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) | [FR-RPT-03.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/228)<br>15/12/2026 |
| [FR-RPT-04](../docs/03-modules/03-management-reporting/requirements/FR-RPT-04.md) Thống kê vấn đề phổ biến | 6 — TC-RPT-04-01…TC-RPT-04-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) | [FR-RPT-04.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/237)<br>16/12/2026 |
| [FR-RPT-05](../docs/03-modules/03-management-reporting/requirements/FR-RPT-05.md) Thống kê khối lượng theo phòng ban | 6 — TC-RPT-05-01…TC-RPT-05-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) | [FR-RPT-05.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/246)<br>17/12/2026 |
| [FR-RPT-06](../docs/03-modules/03-management-reporting/requirements/FR-RPT-06.md) Tổng hợp mức hài lòng | 7 — TC-RPT-06-01…TC-RPT-06-07 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) | [FR-RPT-06.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/253)<br>18/12/2026 |
| [FR-IAM-01](../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-01.md) Đăng nhập | 6 — TC-IAM-01-01…TC-IAM-01-06 | [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) | [FR-IAM-01.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/262)<br>04/11/2026 |
| [FR-IAM-02](../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-02.md) Đăng xuất | 4 — TC-IAM-02-01…TC-IAM-02-04 | [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) | [FR-IAM-02.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/271)<br>05/11/2026 |
| [FR-IAM-03](../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-03.md) Kiểm soát quyền theo vai trò và hồ sơ | 6 — TC-IAM-03-01…TC-IAM-03-06 | [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) | [FR-IAM-03.QA.02](https://github.com/nathyu-le/directorofstudentservice/issues/280)<br>06/11/2026 |

**Tổng:** 24 FR, 149 TC; AC/TC của mỗi FR không có cùng số lượng bắt buộc. Thời gian `.QA.02` là lịch chạy dự kiến từng FR, không phải actual. Mỗi FR còn có `.QA.01` chuẩn bị fixture và `.QA.03` báo cáo/theo dõi bug; đóng task báo cáo không chứng minh mọi bug đã được sửa.

## 6. Quy trình thực thi test chức năng và tích hợp

1. Chọn Test ID, FR/AC và revision; đọc preconditions, data, steps và expected từ file nguồn.
2. Nạp/reset fixture; xác nhận actor, scope, state, version và mốc giờ ban đầu. Lưu số bản ghi/giá trị cần đối chiếu khi case thay đổi dữ liệu.
3. Thực hiện thao tác đúng bước. Case trái quyền gọi API trực tiếp, không chỉ kiểm ẩn nút; case concurrency phải tái hiện thứ tự/lần gửi và version.
4. Đối chiếu toàn bộ expected: response/HTTP, UI, bản ghi, history/version và rollback khi case yêu cầu. Không tự đổi expected giữa lượt chạy.
5. Ghi actual và kết quả; Fail liên kết defect; Blocked ghi nguyên nhân/owner. Lưu evidence đủ tái hiện, không chứa secret hoặc hồ sơ thật.
6. Sau fix có build mới, retest case lỗi và regression theo phân tích tác động. Giữ kết quả cũ và thêm run mới, không xóa dấu Fail để làm báo cáo đẹp.

### 6.1. Smoke trước một lượt test rộng

Khi build có các chức năng tương ứng, dùng một tập nhỏ từ case đã có: đăng nhập hợp lệ IAM-01; gửi yêu cầu STU-01; đọc đúng hồ sơ STU-03; cùng mã xuất hiện ở hàng chờ DSP-01; kiểm quyền IAM-03; đăng xuất IAM-02. Mỗi case vẫn phải dùng ID/expected chính thức. Nếu một luồng hoặc endpoint chưa được bàn giao, chỉ chạy phần có thật và ghi thiếu đầu vào, không tạo kết quả cho phần chưa có.

Smoke xác định có đủ điều kiện tiếp tục hay phải dừng nhánh, không thay thế 149 TC và không phải milestone nghiệm thu riêng.

### 6.2. Ví dụ đọc một case có sẵn

| Trường | Giá trị từ TC-STU-01-01 |
| --- | --- |
| FR / AC | FR-STU-01 / AC-STU-01-01 |
| Tiền điều kiện | Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo. |
| Dữ liệu | Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục. |
| Thao tác chính | Reset fixture; mở luồng gửi yêu cầu và thực hiện POST /api/requests; đọc lại UI, hàng chờ và dữ liệu theo bước nguồn. |
| Expected | Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã. |
| Result hiện trong nguồn | Not Run |

Đây là ví dụ **kế hoạch thực thi**, không phải run record đã chạy. Các trường Actual/Tester/ExecutedAt/Evidence chỉ điền khi thực thi. Những case khác phải dùng dữ liệu và expected riêng của chúng.

### 6.3. Điểm kiểm tích hợp đặc thù

- STU-01 → DSP-01/DSP-02: cùng request_id/reference, chủ lấy từ phiên, không có request thứ hai vì retry cùng ý định.
- DSP-04 → IAM-03/DSP-02: quyền sửa được đọc lại theo người phụ trách hiện hành; người cũ không tiếp tục sửa; scope báo cáo độc lập quyền dispatch.
- DSP-07 → STU-03/STU-04 → DSP-09: câu hỏi mở đọc được đúng chủ, trả lời đúng question, giữ started_at và trách nhiệm theo rule, không giải quyết khi chưa đủ điều kiện.
- DSP-09 → STU-05 → DSP-10 → RPT: result/feedback/lịch sử liên kết đúng hồ sơ; đóng theo quyền và trạng thái, số liệu theo công thức/kỳ của FR.
- Mutation lỗi: không ghi child/history một phần hoặc tăng version sai; version cũ bị từ chối, no_change và replay theo từng FR.

## 7. Kiểm thử hệ thống và phi chức năng

### 7.1. Tám kịch bản E2E

| Mã và mục tiêu | Thực hiện | Expected |
| --- | --- | --- |
| E2E-01 — Yêu cầu đến kết quả | Tạo → phân loại → giao → bắt đầu → tiến độ công khai → giải quyết → phản hồi → đóng. | Một request_id; đúng người/trạng thái/lịch sử; phản hồi tính đúng một lần. |
| E2E-02 — Bổ sung thông tin | Yêu cầu câu hỏi khi đang xử lý; SV đọc/trả lời; NV giải quyết. | Chờ bổ sung → Đang xử lý; giữ hồ sơ và trách nhiệm; không câu hỏi mở khi giải quyết. |
| E2E-03 — Phối hợp đổi trách nhiệm | Giao PB-A/NV-A rồi chuyển PB-B/NV-B có lý do; thử NV-A và NV-B. | Chỉ người hiện hành sửa; lịch sử giữ bộ cũ/mới; không có khoảng dữ liệu hai người. |
| E2E-04 — Hạn và báo cáo | Đặt hạn, đo trước/đúng/sau hạn, giải quyết rồi đo lại. | Đúng công thức quá hạn; yêu cầu kết thúc rời danh sách quá hạn. |
| E2E-05 — Đối chiếu 6 báo cáo | Seed có kết quả đã biết; chạy từng báo cáo và đối chiếu query nguồn. | Mọi tổng, trung bình, mẫu, kỳ và phạm vi khớp định nghĩa; dữ liệu rỗng hiển thị đúng. |
| E2E-06 — Quyền xuyên luồng | Đăng nhập từng vai trò; thử hành động hợp lệ và trái quyền; đăng xuất rồi gọi phiên cũ. | Không lộ hồ sơ/số liệu/ghi chú; phiên cũ không đọc/ghi. |
| E2E-07 — Không bỏ sót/gửi trùng | Gửi n yêu cầu với n khóa khác nhau, gửi lặp một khóa, đối chiếu hàng chờ và DB. | Có n hồ sơ duy nhất, mỗi mã ở hàng chờ trong phạm vi; không phát sinh n+1 do lặp. |
| E2E-08 — Chạy lại để bàn giao | Nhóm khác/một người không trực tiếp viết cài mới theo README và chạy E2E-01. | Chạy được bằng nguồn/seed/tài liệu; lỗi và bước thiếu ghi vào handoff, không giả ký nghiệm thu. |

E2E-01…07 thuộc QA-04; E2E-08 thuộc QA-10 sau khi source/seed/guide sẵn. Các trạng thái được dựng bằng fixture để test FR sớm không được coi là bằng chứng toàn E2E đã chạy.

### 7.2. Sáu kiểm tra NFR

| Mã | Cách kiểm | Expected và giới hạn nguồn | Task |
| --- | --- | --- | --- |
| NFR-SEC-01 | Chạy ma trận quyền, giả role/id, SQL injection/XSS, CSRF thiếu/sai; expected: bị chặn hoặc hiển thị an toàn, không thay dữ liệu. | Mọi API kiểm tra phiên/vai trò/đối tượng; mật khẩu băm; truy vấn tham số hóa; output encode; chống CSRF cho thao tác ghi; không ghi mật khẩu/nội dung nhạy cảm vào log. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-REL-01 | Giả lỗi trước commit, gửi lặp và hai cập nhật cùng phiên bản; expected rollback hoặc một kết quả hợp lệ, không ghi đè. | Nghiệp vụ/lịch sử cùng transaction; kiểm tra phiên bản; gửi lặp không tạo hai yêu cầu/kết quả. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-USAB-01 | Thực hiện gửi/theo dõi/bổ sung bằng hai viewport; ghi lỗi tràn và thao tác bị che; chưa có chứng cứ thì Not Run. | Luồng chính thao tác được ở viewport 1366 × 768 và 390 × 844; nhãn rõ, lỗi tại trường, điều hướng bàn phím, không mất dữ liệu vì lỗi kiểm tra. Các viewport là mục tiêu thử prototype đề xuất. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-PERF-01 | Seed 1.000 hồ sơ tổng hợp; ghi máy, môi trường, thời gian từng lần, p95; không so trên máy khác mà bỏ cấu hình. | Mục tiêu thử đề xuất: 95% trong 20 lần đo trang danh sách/chi tiết/báo cáo ≤3 giây với 1.000 yêu cầu mẫu trên máy demo ghi rõ cấu hình. Chưa là SLA production. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-OBS-01 | Tạo lỗi có kiểm soát và cập nhật hồ sơ; đối chiếu log với sự kiện; tìm dấu mật khẩu/token mẫu, expected không có. | Có request/correlation ID cho lỗi; lịch sử nghiệp vụ có tác nhân/thời gian/hành động, không lộ mật khẩu hay token. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-DEP-01 | Môi trường sạch: tạo DB, import seed, chạy prototype, đăng nhập mẫu và thực hiện E2E-01; ghi bước/sai khác và evidence. Không coi đã deploy production. | README nêu PHP/MySQL phiên bản được nhóm chốt, cấu hình .env.example không secret, schema/seed, cách khởi chạy và kiểm thử; nhóm khác chạy được trên môi trường ghi rõ. | [QA-10](https://github.com/nathyu-le/directorofstudentservice/issues/58) |

Các viewport và mục tiêu hiệu năng trong nguồn là đề xuất cho prototype, còn gắn quyết định môi trường/OQ-07. NFR-SEC-01 là phạm vi kiểm an toàn theo thiết kế, không tuyên bố đã thực hiện một đợt penetration test hoặc chứng nhận bảo mật toàn diện.

Với NFR-PERF-01: chuẩn bị 1.000 hồ sơ tổng hợp; ghi cấu hình, dữ liệu và điều kiện đo. Với từng trang danh sách/chi tiết/báo cáo được đo, lưu 20 thời gian và ghi cách tính p95; đề xuất dùng nearest rank (phần tử thứ 19 của 20 số đã sắp tăng dần) để nhóm review thống nhất trước đo. Đây là quy ước phép đo đề xuất, không thay target nguồn: 95% lần đo ≤3 giây trong điều kiện prototype đã ghi. Không suy thành khả năng chịu 1.000 người dùng đồng thời hoặc production SLA.

Với compatibility/usability: ghi browser/version thật và kiểm ít nhất các viewport nguồn sau khi môi trường được chốt; nhận xét cần chỉ rõ thao tác/ảnh lỗi. Không tự nhận “mọi thiết bị/trình duyệt đều đạt”.

### 7.3. Bốn mục tiêu đo lường sản phẩm

| Mã và mục tiêu | Công thức hoặc điều kiện nguồn | Task |
| --- | --- | --- |
| MET-01 — Có người chịu trách nhiệm | M/N>=90%, N>0; M requests có bộ department/assignee hợp lệ. | [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) |
| MET-02 — Theo dõi trạng thái | K/N>=90%, N>0; K hồ sơ chủ SV đọc được và khớp DB/lifecycle. | [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) |
| MET-03 — Giảm hỏi lại tiến độ | (B-A)/B>=30%, B>0; kịch bản trước/sau tương đương, tập yêu cầu tương đương. | [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) |
| MET-04 — Giảm bỏ sót/trùng | Đếm intent/request/hàng chờ trước/sau; không tự đặt ngưỡng% mới. | [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) |

Ghi dataset/seed, `as_of`, phạm vi mẫu, truy vấn/nguồn, số đếm và phép tính. N=0/B=0 hoặc chưa có mẫu thì không kết luận đạt hay tự gán 0%; ghi tình trạng thiếu điều kiện. MET-04 không có ngưỡng phần trăm mới. Kết quả kịch bản giả lập chỉ có giá trị cho mẫu kiểm, không chứng minh hiệu quả vận hành thật của toàn trường.

## 8. Quản lý kết quả và vòng đời defect

### 8.1. Trạng thái của test case trong một lượt chạy

| Result | Điều kiện dùng | Hồ sơ bắt buộc |
| --- | --- | --- |
| Not Run | Chưa thực thi case trên build/phạm vi đang báo cáo | Giữ rõ chưa chạy; không dùng ngày Master để đổi kết quả |
| Pass | Đã thực hiện đủ bước và actual khớp mọi expected của case | Build, môi trường, seed, actor, tester/ngày thật, actual và evidence |
| Fail | Đã chạy nhưng có ít nhất một expected không đạt | Thông tin trên cùng defect ID, tác động và bằng chứng sai lệch |
| Blocked | Xác định có trở ngại khiến không thể tiếp tục/hoàn tất case | Trở ngại, bước bị dừng, owner gỡ và dependency; không tính là Pass |

Kết quả gắn build; bản sửa hoặc cấu hình mới có thể yêu cầu run mới. Một số bước đạt nhưng case chưa đủ bước/expected không được ghi Pass.

### 8.2. Nội dung defect report

Ghi ID/issue URL; tiêu đề nêu hành vi lỗi; FR/AC/TC; build; môi trường; fixture/actor; tiền điều kiện; từng bước tái hiện; tần suất; expected và actual tách biệt; severity và tác động; evidence; owner; ưu tiên do QA/PM thống nhất; PR/commit fix; run retest/regression và kết luận. Không chỉ ghi “bị lỗi” hoặc “UI sai”. Defect về yêu cầu mâu thuẫn phải liên kết OQ/CR, không ép dev sửa theo một expected chưa xác định.

### 8.3. Phân loại severity và priority

Bảng sau là **quy ước đề xuất để review áp dụng**, không khẳng định repo đã có nhãn hoặc chính sách severity này. Severity nói mức ảnh hưởng; priority nói thứ tự xử lý theo tác động, dependency, workaround và cổng bị chặn. Không suy ra SLA sửa lỗi từ severity.

| Severity | Tiêu chí đánh giá | Ví dụ tình huống giả định thuộc scope |
| --- | --- | --- |
| Critical | Sai quyền/lộ dữ liệu, mất hoặc hỏng dữ liệu, hoặc luồng cốt lõi không thể sử dụng và không có workaround chấp nhận được | Sinh viên đọc hồ sơ người khác; mutation ghi dữ liệu/history không nhất quán |
| Major | Một hành vi hoặc kết quả nghiệp vụ chính sai, ảnh hưởng đáng kể dù có thể còn workaround | Phân công sai người; số liệu báo cáo tính sai; không giải quyết được yêu cầu hợp lệ |
| Minor | Ảnh hưởng giới hạn, có workaround, không làm sai quyền/toàn vẹn dữ liệu hay chặn luồng chính | Một nhãn/hướng dẫn chưa rõ nhưng vẫn thao tác đúng; cần xem AC trước khi kết luận |
| Cosmetic | Lỗi trình bày, không ảnh hưởng ý nghĩa, đọc hiểu và thao tác | Khoảng cách/căn lề không nhất quán trong điều kiện đã kiểm |

Phân loại theo tác động quan sát được, không dựa tên component: lỗi UI gây mất dữ liệu không phải Cosmetic. Mọi lỗi chặn luồng, sai quyền hoặc sai/mất dữ liệu đều chặn cổng theo baseline, bất kể nhãn severity được chọn. Không có quy tắc tự động cho Minor/Cosmetic được bỏ qua AC.

### 8.4. Theo dõi fix và xác nhận lại

Luồng xử lý hồ sơ đề xuất: **New → Triaged → In Progress → Resolved → Ready for Retest → Closed**; retest thất bại thì **Reopened → In Progress**. `Resolved` chỉ nghĩa Dev cung cấp bản sửa; QC đóng sau retest Pass và đã xử lý regression cần thiết. Đây là trạng thái logic của hồ sơ, chưa được cấu hình như custom status trên GitHub. Trạng thái issue Open/Closed không thay bản ghi chi tiết.

Bug duplicate phải dẫn defect gốc và giữ liên kết case; rejected/by design phải có căn cứ PRD và quyết định review. Không xóa kết quả test Fail ban đầu. Bug còn mở, chấp nhận rủi ro hoặc trì hoãn đều phải xuất hiện trong report và quyết định cổng.

## 9. Retest, regression, tạm dừng và chạy lại

**Retest/confirmation:** chạy lại case từng Fail trên build đã sửa với dữ liệu tái hiện phù hợp; lưu actual/evidence mới và liên kết defect. **Regression:** kiểm tác động ngoài lỗi đó, theo thay đổi về quyền, dữ liệu, vòng đời, API, UI và báo cáo. Hai việc không đồng nghĩa.

| Thay đổi | Phạm vi regression cần đánh giá |
| --- | --- |
| Session/quyền/scope | IAM, hành vi đọc/ghi STU/DSP, report scope, internal/public và logout cũ |
| Transaction/version/retry | Mutation bị đổi, history/child, case đồng thời/stale và hành vi replay/no_change liên quan |
| Assignment/state/question/result | Quyền người cũ/mới, vòng bổ sung, giải quyết/đóng, tiến độ/lịch sử và số liệu bị tác động |
| Query/filter/công thức/kỳ | Báo cáo bị sửa và báo cáo dùng chung tập nguồn; dữ liệu rỗng, biên ngày và join nhiều bản ghi |
| UI/API contract hoặc môi trường | Luồng/màn hình dùng contract, xử lý lỗi, viewport; smoke và kiểm chạy lại khi cấu hình/gói thay đổi |

QA-07 là cửa sổ retest/regression theo Master; lỗi phát hiện trước đó vẫn phải được quản lý và xác minh trước khi kết luận FR/cổng. Không mặc định mọi fix được chờ tới cuối dự án hoặc đã nằm trong giờ `.QA.03`.

**Tạm dừng nhánh test** khi môi trường không ổn định, seed không tái lập, build không xác định, lỗi chặn làm kết quả sau đó không đáng tin, phát hiện lộ dữ liệu/secret hoặc expected bị mâu thuẫn. Ghi điểm dừng, case chịu ảnh hưởng, defect/OQ và owner. Không buộc dừng nhánh độc lập có đủ điều kiện.

**Tiếp tục** khi owner cung cấp bản sửa/đầu vào/định nghĩa đã review, xác nhận lại build/seed, smoke phù hợp đạt và xác định case cần chạy lại. PM đánh giá ảnh hưởng ngày cổng; QC không tự dời Master để che chậm tiến độ.

## 10. Automation và công cụ

Hiện repo chưa có framework test, test scripts hoặc workflow CI; vì vậy không ghi rằng mỗi commit đã được tự động kiểm thử, có coverage hoặc pipeline Pass. Thực thi thủ công có bước/evidence tái lập là cách có thể áp dụng khi build sẵn.

Khi nhóm bổ sung automation, ưu tiên kiểm lặp ổn định và rủi ro cao: validation/rule nhỏ ở unit, quyền API, retry/version và regression phép tính báo cáo. UI automation chỉ sau khi luồng/fixture ổn định. Nhóm chọn công cụ phù hợp mã thực tế; bài mẫu nêu Selenium/Cypress/Playwright không có nghĩa dự án phải dùng hoặc đã dùng chúng.

Mỗi script cần liên kết FR/TC hoặc kiểm soát bổ sung được review, fixture/reset, expected, exit code và log/run gắn commit. CI nếu được thêm phải chỉ rõ trigger, các job/check cần chạy và cách xử lý lỗi; Dev review cấu hình, QA kiểm tuân thủ. Thêm script không tự tăng số TC baseline; tạo case mới phải cập nhật mapping có kiểm soát. Effort/ngày phát sinh ngoài task hiện có phải được PM đánh giá trước.

## 11. Lịch thực hiện và quan hệ phụ thuộc

Ngày dưới đây lấy từ Master v6.0, là dự kiến. Tiền tố `QA-` là mã task có sẵn của vai trò; không đổi thành `QC-` vì sẽ làm đứt liên kết. Đối với task từng FR dùng lịch tại mục 5 và manifest.

| Task và đầu ra | Lịch dự kiến | Predecessor trong Master |
| --- | --- | --- |
| [QA-01](https://github.com/nathyu-le/directorofstudentservice/issues/11) Rà AC và ma trận kiểm thử | 14/10/2026 | [PM-02](https://github.com/nathyu-le/directorofstudentservice/issues/2) |
| [QA-02](https://github.com/nathyu-le/directorofstudentservice/issues/12) Chuẩn hóa case, expected và fixture nền | 15/10/2026–16/10/2026 | [QA-01](https://github.com/nathyu-le/directorofstudentservice/issues/11), [BE-02](https://github.com/nathyu-le/directorofstudentservice/issues/7) |
| [QA-03](https://github.com/nathyu-le/directorofstudentservice/issues/51) Kiểm bằng chứng bản tiếp nhận đã tích hợp | 13/11/2026 | [FE-03](https://github.com/nathyu-le/directorofstudentservice/issues/45), [FR-STU-01.QA.03](https://github.com/nathyu-le/directorofstudentservice/issues/67), [FR-STU-02.QA.03](https://github.com/nathyu-le/directorofstudentservice/issues/73), [FR-STU-03.QA.03](https://github.com/nathyu-le/directorofstudentservice/issues/85), [FR-DSP-01.QA.03](https://github.com/nathyu-le/directorofstudentservice/issues/112), [FR-DSP-02.QA.03](https://github.com/nathyu-le/directorofstudentservice/issues/121) |
| [QA-04](https://github.com/nathyu-le/directorofstudentservice/issues/52) Chạy E2E-01 đến E2E-07 xuyên module | 28/12/2026 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) Chạy NFR, trừ kiểm tra chạy lại khi bàn giao | 29/12/2026 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FE-05](https://github.com/nathyu-le/directorofstudentservice/issues/48) Rà mobile, bàn phím và khả năng sử dụng | 28/12/2026 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FE-06](https://github.com/nathyu-le/directorofstudentservice/issues/50) Sửa lỗi giao diện trước regression | 29/12/2026 | [FE-05](https://github.com/nathyu-le/directorofstudentservice/issues/48), [QA-04](https://github.com/nathyu-le/directorofstudentservice/issues/52) |
| [BE-06](https://github.com/nathyu-le/directorofstudentservice/issues/42) Xử lý phát hiện từ kiểm tra phi chức năng | 30/12/2026 | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) Đo MET-01 đến MET-04 trên kịch bản giả lập | 31/12/2026 | [QA-04](https://github.com/nathyu-le/directorofstudentservice/issues/52), [BE-06](https://github.com/nathyu-le/directorofstudentservice/issues/42) |
| [BE-07](https://github.com/nathyu-le/directorofstudentservice/issues/46) Sửa lỗi backend trước regression | 31/12/2026–01/01/2027 | [BE-06](https://github.com/nathyu-le/directorofstudentservice/issues/42), [QA-04](https://github.com/nathyu-le/directorofstudentservice/issues/52) |
| [QA-07](https://github.com/nathyu-le/directorofstudentservice/issues/56) Retest, regression và rà kết quả MET | 04/01/2027–05/01/2027 | [BE-07](https://github.com/nathyu-le/directorofstudentservice/issues/46), [FE-06](https://github.com/nathyu-le/directorofstudentservice/issues/50), [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) |
| [FE-07](https://github.com/nathyu-le/directorofstudentservice/issues/49) Hoàn thiện hướng dẫn thao tác để chạy lại | 30/12/2026 | [FE-06](https://github.com/nathyu-le/directorofstudentservice/issues/50) |
| [BE-08](https://github.com/nathyu-le/directorofstudentservice/issues/44) Đóng gói source, seed và hướng dẫn chạy | 06/01/2027 | [QA-07](https://github.com/nathyu-le/directorofstudentservice/issues/56) |
| [QA-10](https://github.com/nathyu-le/directorofstudentservice/issues/58) Kiểm chạy lại E2E-08 và NFR-DEP-01 | 07/01/2027 | [BE-08](https://github.com/nathyu-le/directorofstudentservice/issues/44), [FE-07](https://github.com/nathyu-le/directorofstudentservice/issues/49) |
| [QA-09](https://github.com/nathyu-le/directorofstudentservice/issues/57) Chốt test report và hồ sơ kết quả | 08/01/2027 | [QA-10](https://github.com/nathyu-le/directorofstudentservice/issues/58) |
| [QA-08](https://github.com/nathyu-le/directorofstudentservice/issues/55) Rà checklist nghiệm thu và vấn đề còn mở | 11/01/2027 | [QA-09](https://github.com/nathyu-le/directorofstudentservice/issues/57) |
| [PM-08](https://github.com/nathyu-le/directorofstudentservice/issues/40) Rà bằng chứng và kết luận sẵn sàng nghiệm thu | 12/01/2027 | [QA-08](https://github.com/nathyu-le/directorofstudentservice/issues/55), [QA-09](https://github.com/nathyu-le/directorofstudentservice/issues/57) |
| [PM-09](https://github.com/nathyu-le/directorofstudentservice/issues/38) Trình bày và bàn giao bản đã kiểm chứng | 13/01/2027 | [PM-08](https://github.com/nathyu-le/directorofstudentservice/issues/40) |

QA-04 và QA-05 cùng phụ thuộc MS-05; không tự thêm quan hệ QA-04 → QA-05. Nhánh FE-05/06/07 có thể tiến hành theo input của nó. QA-06 cần QA-04 và BE-06; QA-07 cần BE-07, FE-06 và QA-06. QA-10 cần BE-08 và FE-07; sau đó QA-09 → QA-08 → PM-08 → PM-09. Các dependency đầy đủ giữ theo bảng và manifest, không thay bằng một chuỗi tuần tự chung.

MS-06 có hạn 22/01/2027, còn các task review/bàn giao đang dự kiến hoàn tất trước hạn đó. Hạn milestone không phải bằng chứng đã hoàn thành hoặc ngày thực tế. Lịch dựa trên giả định nguồn lực còn phải được xác nhận; không suy availability từ ô Gantt trống.

## 12. Kiểm chứng cổng và UAT

| Mốc | Ngày dự kiến | Phạm vi | Điều kiện đầu ra từ kế hoạch |
| --- | --- | --- | --- |
| [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) Truy cập hệ thống đúng quyền | 06/11/2026 | FR-IAM-01, FR-IAM-02, FR-IAM-03<br>3 FR / 16 TC | Đăng nhập/đăng xuất hoạt động; phiên hết hiệu lực bị từ chối; kiểm quyền và phạm vi dữ liệu trên máy chủ, không chỉ ẩn nút. Tài khoản/danh mục dùng seed, không có chức năng tạo tài khoản mới. |
| [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) Tiếp nhận và theo dõi yêu cầu | 16/11/2026 | FR-STU-01, FR-STU-02, FR-STU-03, FR-DSP-01, FR-DSP-02<br>5 FR / 26 TC | Sinh viên gửi yêu cầu hợp lệ, thấy đúng hồ sơ của mình; điều phối thấy cùng mã trong hàng chờ và hồ sơ xử lý. Kiểm gửi lặp, validation, quyền đọc và lịch sử công khai/nội bộ theo AC. Các trạng thái chưa có thao tác sinh dữ liệu được kiểm bằng fixture; không coi đó là bằng chứng E2E đã chạy. |
| [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) Điều phối và phối hợp xử lý | 04/12/2026 | FR-DSP-03, FR-DSP-04, FR-DSP-05, FR-DSP-06, FR-DSP-07, FR-DSP-08, FR-STU-04<br>7 FR / 50 TC | Phân loại, phân công, đặt hạn, bắt đầu xử lý, cập nhật tiến độ và vòng yêu cầu/bổ sung thông tin hoạt động đúng vai trò, scope, state và version. Kiểm quyền sau đổi người phụ trách, không ghi dữ liệu/lịch sử một phần. |
| [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) Trả kết quả và hoàn tất hỗ trợ | 09/12/2026 | FR-DSP-09, FR-STU-05, FR-DSP-10<br>3 FR / 20 TC | Ghi kết quả, sinh viên đọc kết quả và phản hồi; đóng yêu cầu theo đúng điều kiện trong PRD. Không thêm email, SMS hay xuất file. Xác nhận đọc tiến độ/lịch sử liên quan không bị hồi quy. |
| [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) Theo dõi và báo cáo đúng dữ liệu | 18/12/2026 | FR-RPT-01, FR-RPT-02, FR-RPT-03, FR-RPT-04, FR-RPT-05, FR-RPT-06<br>6 FR / 37 TC | Sáu báo cáo đúng tập nguồn, phạm vi quyền, bộ lọc, biên thời gian và công thức PRD; đối chiếu với fixture có expected xác định. Bản tích hợp đủ 24 FR; không có chức năng xuất dữ liệu ngoài baseline. |
| [MS-06](https://github.com/nathyu-le/directorofstudentservice/milestone/6) Kiểm chứng và bàn giao chạy lại được | 22/01/2027 | Toàn bộ 24 FR / 149 TC + 18 kiểm tra hệ thống | Có kết quả thật của 149 TC chức năng và 18 kiểm tra hệ thống; đối chiếu toàn bộ AC. Không còn lỗi chặn luồng, sai quyền hoặc làm mất/sai dữ liệu; lỗi khác phải có ghi nhận tác động và quyết định chấp nhận. Có build/commit, test report, source/seed/guide và bằng chứng người khác chạy lại. Kết quả MET dưới mục tiêu phải được báo cáo, không tự coi đạt; xác nhận nghiệm thu bởi người có thẩm quyền, không giả chữ ký. |

### 12.1. Điều kiện kết thúc một FR

Build tích hợp có source/PR review; toàn bộ AC/TC của FR có kết quả đạt và bằng chứng; defect liên kết, không còn lỗi chặn. Case chưa chạy, Blocked hoặc Fail không được đổi thành Pass bằng chấp nhận rủi ro. Nếu cần đổi AC/phạm vi, đi qua CR rồi mới áp dụng baseline mới.

### 12.2. Điều kiện MS-06 và chấp nhận nghiệp vụ

- Có hồ sơ đầy đủ cho 149 TC chức năng và 18 kiểm tra hệ thống trên build được xác định; mọi kết luận có actual/evidence và giới hạn.
- Không còn lỗi chặn luồng, sai quyền hoặc làm mất/sai dữ liệu; lỗi khác có tác động và quyết định chấp nhận thật, không tự ghi TC đạt.
- Có source, schema/seed, cấu hình mẫu không secret, user/setup guide và kết quả chạy lại E2E-08/NFR-DEP-01.
- Người đại diện nghiệp vụ được chỉ định review các luồng đã có: gửi/theo dõi; điều phối/bổ sung; trả kết quả/phản hồi/đóng; báo cáo; và cách chạy lại. QC chuẩn bị dữ liệu/hướng dẫn và ghi vấn đề; người có thẩm quyền ghi quyết định UAT/nghiệm thu, không để tester tự ký thay.
- OQ ảnh hưởng hành vi nghiệm thu phải có quyết định phù hợp; đạt một thiết kế đề xuất không chứng minh quyết định đó đã được chấp thuận.

QA-08 và PM-08 dùng hồ sơ trên để review, PM-09 gắn bàn giao đúng build. Nếu code/config/seed thay đổi sau QA-10, đánh giá tác động và chạy lại kiểm tra cần thiết trước khi dùng hồ sơ cũ để bàn giao. Đây là nghiệm thu prototype, không tự chuyển thành go-live production.

## 13. Run record và báo cáo QC

### 13.1. Mẫu thông tin một lượt chạy

```text
Run ID / Test ID / FR và AC / mức kiểm thử
Build hoặc commit / môi trường / schema và fixture revision / actor và scope
Preconditions / dữ liệu / bước đã thực hiện / expected theo revision
Actual quan sát được / Result / ExecutedAt / Tester
Evidence URL / Defect URL / build fix / retest run / regression scope và kết quả
Giới hạn hoặc blocker / người review khi có thật
```

Mỗi lần chạy lại thêm kết quả gắn build, không ghi đè mất lịch sử. Screenshot cần chỉ ra thao tác/actor/phạm vi; case dữ liệu/transaction cần bổ sung response/log hoặc đối chiếu DB phù hợp. Bằng chứng phải được làm sạch thông tin nhạy cảm. Lưu ở task/report đã thống nhất và dẫn link, không yêu cầu một công cụ trả phí mới.

### 13.2. Nội dung báo cáo tại `.QA.03`, QA-09 và review cổng

1. Phạm vi, revision/build, môi trường, seed, kỳ chạy và người thực hiện.
2. Danh sách case theo từng FR và từng nhóm E2E/NFR/MET; số Pass/Fail/Blocked/Not Run từ record, không chỉ một tỷ lệ tổng.
3. Defect mở/đã sửa/đã retest, tác động, owner, phần regression đã kiểm và chưa kiểm.
4. NFR/MET có số đo gốc, công thức, cấu hình, mẫu, `as_of` và giới hạn suy luận.
5. Mức đáp ứng entry/exit criteria, OQ/ngoại lệ, kết luận sẵn sàng kỹ thuật và hành động còn thiếu.
6. Liên kết hồ sơ cho QA/PM và quyết định nghiệp vụ riêng khi có thật.

Tỷ lệ đã thực thi = (Pass + Fail) / tổng case trong phạm vi. Tỷ lệ đạt trong số đã thực thi = Pass / (Pass + Fail). Luôn báo số Blocked/Not Run và mẫu số; mẫu số 0 thì N/A. Nếu đã quan sát một expected không đạt thì giữ Fail dù bước sau bị chặn và ghi thêm blocker. Chỉ ghi Blocked khi trở ngại khiến chưa đủ căn cứ kết luận Pass/Fail; nêu bước đã chạy. Báo tách 149 TC chức năng, 8 E2E, 6 NFR và 4 MET; nếu tổng hợp thì ghi rõ tập 167 kiểm tra, tránh gọi cả 167 là TC chức năng hoặc code coverage.

Không có dữ liệu run thì báo chưa thực thi, không đặt số defect bằng 0 hay khẳng định sản phẩm không có lỗi. Defect density/reopen/leakage là chỉ số quy trình do QA tổng hợp theo định nghĩa ở QA.md; QC cung cấp defect/run history làm nguồn, không tính bằng số issue task kế hoạch.

## 14. Nguồn và quản lý cập nhật

- [PRD và task manifest v6.0](../docs/05-project/requirements.json), [phạm vi dự án](../docs/01-overview/scope.md).
- [Vòng đời](../docs/02-domain/lifecycle.md), [quyền](../docs/02-domain/permissions.md), [hợp đồng API](../docs/02-domain/api-contract.md), [từ điển dữ liệu](../docs/02-domain/data-dictionary.md).
- [Ma trận FR–AC–TC](../docs/04-acceptance/traceability-matrix.md), [149 TC chi tiết](../docs/04-acceptance/test-cases.md), [18 kiểm tra hệ thống](../docs/04-acceptance/system-tests.md).
- [Master Gantt](../UniSupport_Master_Plan.xlsx), [điều kiện milestone](../docs/05-project/master-alignment.md), [Team Charter](../TeamCharter.md), [câu hỏi mở](../docs/05-project/open-questions.md).
- [Kế hoạch QA](QA.md).
- ASQ, [Quality Assurance vs Quality Control](https://asq.org/quality-resources/quality-assurance-vs-control): tham chiếu phân biệt QA và QC.
- ISTQB, [CTFL Syllabus v4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf), mục 2.2 và 5.5: tham chiếu thuật ngữ test level/type, confirmation/regression và defect; không phải chứng nhận UniSupport tuân thủ toàn bộ tiêu chuẩn.
- Đoạn QA/QC người dùng cung cấp ngày 04/11/2026: dùng cấu trúc unit → integration → system → UAT, automation và vòng phản hồi về QA; điều chỉnh theo scope và bằng chứng repo.

Khi upstream thay đổi, cập nhật phần QC bị ảnh hưởng, giữ mã nguồn chuẩn và liên kết CR/revision; QA kiểm lại consistency. Tài liệu này không tự sửa PRD, Master, issue hay milestone để khớp một giả định mới.
