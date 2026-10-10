# UniSupport — Team Charter v5.1

Trạng thái: dự thảo để nhóm thống nhất áp dụng. Nguồn yêu cầu: Proposal v6 và PRD v5.1. Tài liệu này quy định cách phối hợp và quy trình thực hiện, không thay PRD hoặc Master Plan. Tên thành viên, kênh chat và khung giờ làm việc chưa xác nhận được để trống.

## 1. Phân vai và đầu mối

| Vai trò theo proposal | Việc chịu trách nhiệm | Người review đầu ra |
| --- | --- | --- |
| PM/BA | Làm rõ yêu cầu, quản lý PRD/AC, chuẩn bị task, sắp phụ thuộc và lịch, ghi quyết định/thay đổi, kiểm điều kiện hoàn thành | BE/FE/QA review đặc tả và lịch liên quan |
| BE | Schema/seed, query/API, session/quyền, transaction và tính nhất quán, đóng gói môi trường chạy | FE review contract/kết nối; QA kiểm hành vi và dữ liệu |
| FE | Màn hình desktop/mobile, trạng thái giao diện, validation, kết nối API và hiển thị lỗi | BE review contract; QA kiểm thao tác và khả năng sử dụng |
| QA/tài liệu | Fixture/case, thực thi và evidence, bug/retest/regression, phép đo mục tiêu và hướng dẫn chạy lại | PM review nội dung; BE/FE review môi trường và tính tái lập |

Mỗi task có một owner. Reviewer không thay owner; người viết không tự xác nhận review của mình. Điều phối là quyền nghiệp vụ trong prototype, không thêm người hoặc vai trò dự án ngoài bốn vai trò trên. PM lấy xác nhận từ thầy/bên có thẩm quyền khi thay phạm vi hoặc cam kết, không ký thay.

## 2. Quy tắc nhận việc và cập nhật

1. Chỉ nhận task đủ mã, đầu vào, đầu ra, FR/AC/TC liên quan, owner, reviewer, estimate, ngày mục tiêu và predecessor. Chưa có input thì ghi câu hỏi/blocker, không tự bịa quy tắc để code.
2. Mỗi người ưu tiên hoàn tất một việc triển khai trước khi mở việc khác. Nếu cần làm song song, PM kiểm giờ theo ngày và dependency trước khi giao. Có task kéo dài trên Gantt không có nghĩa làm toàn thời gian mỗi ngày.
3. Owner cập nhật sau mỗi phiên: phần đã làm, link đầu ra/build/PR, giờ thực tế, phần còn lại, blocker và dự báo. Không điền actual start/finish, Pass hoặc xác nhận review trước khi xảy ra.
4. Ngày mục tiêu giữ để thấy trễ; forecast và actual ghi riêng. Không đẩy due date âm thầm để xóa dấu trễ. Đổi baseline cần quyết định và phiên bản.
5. Dùng dữ liệu giả lập. Không đưa thông tin sinh viên thật, secret hoặc mật khẩu vào repository/task/evidence.

Mẫu cập nhật task: `Task ID / build hoặc PR / đã làm / actual hours / còn lại / blocker / người cần hỗ trợ / forecast finish`.

## 3. Quy trình từ yêu cầu đến hoàn thành

### 3.1 Làm rõ và chuẩn bị

PM lấy FR từ `docs/08-project/requirements.json` v5.1, đối chiếu Markdown và proposal. BE/FE/QA review đầu vào, quyền, trạng thái, trường dữ liệu, lỗi và AC. Câu hỏi chưa đủ nguồn ghi vào Open Questions, không coi thiết kế đề xuất là quyết định đã được sponsor duyệt.

Mỗi FR là parent không tính giờ riêng. Task triển khai gồm:

| Task con | Owner | Đầu vào | Đầu ra và điều kiện chuyển bước |
| --- | --- | --- | --- |
| `.PM.01` | PM | Proposal, FR, câu hỏi mở | Đặc tả và contract có nhận xét BE/FE/QA; các quyết định chưa chốt được chỉ ra |
| `.BE.01` | BE | Contract, schema/seed | Query hoặc mapping dữ liệu đúng FR và index/constraint cần thiết |
| `.BE.02` | BE | Dữ liệu ở BE.01 | Endpoint/logic theo từng bước, trạng thái và công thức của FR |
| `.BE.03` | BE | Endpoint và middleware | Quyền, validation, concurrency/retry/rollback phù hợp; self-check và PR review |
| `.FE.01` | FE | FR, wireframe, contract | Màn hình, dữ liệu nhập/đọc, trạng thái loading/empty/error và thao tác đúng quyền |
| `.FE.02` | FE | UI và backend đã review | Kết nối thật, xử lý HTTP theo contract, self-check desktop/mobile; không lấy mock làm bằng chứng tích hợp |
| `.QA.01` | QA | TC/AC, build và seed | Fixture, actor và dữ liệu biên của FR được chuẩn bị, khả năng chạy được xác nhận |
| `.QA.02` | QA | Build tích hợp và fixture | Toàn bộ TC thuộc FR có actual/result/evidence; Fail tạo bug, Blocked có nguyên nhân |
| `.QA.03` | QA | Kết quả QA.02 | Báo cáo và liên kết bug/retest; lỗi chưa sửa ghi mở, không giả định đã retest |

Các package lịch gom task cùng role/FR theo thứ tự nội bộ. Giờ package là tổng task con, không cộng thêm vào effort. Task cùng package có thể cùng ngày nhưng phải thực hiện tuần tự. Giữa package dùng Finish-to-Start; không bắt QA chạy trước khi build/fixture sẵn sàng. Đầu ra dùng chung như setup, E2E/NFR/MET và handoff nằm riêng trong Master, không tạo module thứ năm.

### 3.2 Triển khai và review

Owner xác nhận đã hiểu task và input. BE/FE thống nhất contract trước khi tách việc. Branch/PR mang mã task, ví dụ `feat/FR-STU-01-BE-02`. PR ghi FR, phạm vi sửa, cách chạy, kết quả tự kiểm và phần chưa hoàn tất.

Reviewer kiểm theo đặc tả và AC, ghi nhận xét trên PR/task. Owner sửa và gửi lại. Review xong mới merge/tích hợp. BE/FE cung cấp build ID, seed revision và cấu hình cho QA; review code không thay thực thi test.

### 3.3 Kiểm thử, bug và retest

QA reset fixture để case trước không ảnh hưởng case sau, thực hiện đúng actor/data/steps. Case quyền gọi API trực tiếp; case ghi kiểm dữ liệu/version/history; case báo cáo đối chiếu tập nguồn và biên kỳ. Ghi một trong bốn giá trị:

- `Not Run`: chưa chạy.
- `Pass`: đã chạy và actual đáp ứng expected đầy đủ.
- `Fail`: đã chạy nhưng khác expected, có bug ID.
- `Blocked`: chưa chạy được, có nguyên nhân và người cần hỗ trợ.

Bug ghi `FR / TC / build / môi trường / dữ liệu / bước tái hiện / expected / actual / severity / evidence / owner / retest`. QA và PM thống nhất ưu tiên theo ảnh hưởng. BE/FE sửa, review, cung cấp build mới; QA retest case lỗi rồi regression phần liên quan. Bug chỉ đóng sau retest đạt. Task QA.03 ban đầu là ghi nhận và theo dõi, không giả rằng mọi lỗi đã được sửa trong giờ đó; sửa lỗi có task/budget riêng hoặc change request khi vượt dự toán.

### 3.4 Điều kiện hoàn thành

Task đặc tả/tài liệu cần link phiên bản và review. Task BE/FE cần source/PR, review, build và self-check. Parent FR chỉ hoàn tất khi triển khai tích hợp, mọi AC/TC của FR đã đạt và không còn lỗi chặn. Số TC của mỗi FR lấy từ manifest, không mặc định bốn case.

Mốc W7/W11/W14/W15 có checklist đầu ra và evidence. Hoàn thành nội bộ không đồng nghĩa bên duyệt đã nghiệm thu. Các phép đo MET dùng kịch bản giả lập, không công bố như kết quả vận hành thật của trường.

## 4. Xử lý chặn và điều chuyển nhân sự

Blocker báo ngay khi biết: task, input thiếu, người cần hỗ trợ, việc phía sau bị ảnh hưởng, phương án và ngày dự báo. PM phản hồi trong phiên làm việc kế tiếp theo lịch nhóm chốt. Việc độc lập đủ input vẫn tiếp tục; không phá dependency để đánh dấu xong.

Trước khi cho mượn BE/FE hoặc người khác:

1. Owner báo lịch tham gia thật, giờ dự án khác, nghỉ, task còn lại và giờ cần giữ theo từng ngày trong task hoặc log nhóm. Ô chưa có dữ liệu không phải số 0.
2. PM đối chiếu Start/Due, giờ còn lại, predecessor trong PRD `plan.tasks` và các mốc trên Master Gantt. Khoảng trống trên Gantt không chứng minh người đó rảnh.
3. PM tính phần giờ có thể điều chuyển bằng lịch thật đã xác nhận; rà ảnh hưởng tới BE/FE/QA nối tiếp. Excel hiện không có mô hình capacity hoặc what-if tự tính.
4. Chốt việc bàn giao, thời gian quay lại, task/mốc bị ảnh hưởng và người xác nhận; sau xác nhận mới điều chỉnh việc ngoài dự án.
5. Nếu đổi baseline/ngày/giờ, cập nhật Gantt, PRD plan.tasks và CSV cùng phiên bản. Rà toàn bộ task nối tiếp và milestone; CSV đã tải không tự cập nhật.

Mẫu đề nghị: `Role/người / ngày từ–đến / giờ mỗi ngày / task đang làm / task bị ảnh hưởng / mốc có nguy cơ / cách bàn giao / người xác nhận / ngày quay lại`.

## 5. Giao tiếp và quyết định

Nhóm tự xác nhận kênh chat, giờ tham gia và thời gian phản hồi. Đề xuất họp đầu tuần 20 phút để chốt input/capacity/dependency, cuối tuần 20 phút để review đầu ra/bug/forecast. Đây chưa là lịch mời đã được gửi.

PM ghi quyết định sau họp/chat vào log và task, gồm người xác nhận, lý do, phiên bản, owner và hạn. Vấn đề FR/AC/scope hỏi PM; API/DB/quyền hỏi BE; UI/kết nối hỏi FE; fixture/result/bug hỏi QA. Nếu vắng, owner bàn giao task/branch/build/input/blocker, PM xác nhận người tiếp nhận; không mặc định có người rảnh thay.

Thay đổi hành vi có mã `CR-xx`: nguồn yêu cầu, FR/AC/TC liên quan, phương án, effort, resource và mốc bị ảnh hưởng. Review tác động rồi mới cập nhật PRD, manifest, tests, Master và Charter nếu quy trình đổi. Giữ mã FR cũ nếu cùng hành vi; không đổi tên/mã tùy ý làm mất liên kết bug/test. Bất đồng phải nêu căn cứ và tác động, PM điều phối; phần cần quyết định giữ Blocked tới khi có xác nhận.

## 6. Lưu đầu ra và đồng bộ

GitHub giữ PRD/source/Charter và lịch sử review. ClickUp giữ công việc/owner/date/estimate/status; Excel chỉ giữ Master Gantt theo ngày, task/FR, role và estimate; không có cột trạng thái, sheet capacity hoặc tính điều chuyển. QA evidence gắn TC/build/date/result. Chưa push/import không ghi như đã thực hiện.

CSV có mã parent/task con và predecessor trong mô tả. Sau import phải tạo native dependencies theo predecessor trong PRD `plan.tasks` và mô tả CSV; văn bản ID không phải link dependency. Gán assignee theo thành viên thật, không gán chủ workspace làm cả bốn role. QA Result phải dùng enum/text, không checkbox. Trạng thái workflow đề xuất: Chưa sẵn sàng → Sẵn sàng → Đang làm → Chờ review → Sẵn sàng QA → Đang QA → Hoàn tất; mapping theo statuses mà workspace thực sự có.

## 7. Xác nhận áp dụng

| Vai trò | Họ tên | Lịch tham gia/kênh liên lạc | Ngày xác nhận | Link xác nhận |
| --- | --- | --- | --- | --- |
| PM/BA |  |  |  |  |
| BE |  |  |  |  |
| FE |  |  |  |  |
| QA/tài liệu |  |  |  |  |

Các ô trên để trống tới khi có xác nhận thật. Trần ngân sách giờ là 288h chính + 32h dự phòng. Giờ dự phòng chưa giao task; chỉ dùng khi PM ghi nhận nguyên nhân, owner, tác động chi phí/mốc và quyết định. Phân bổ giờ theo task và ngày trong Master vẫn là đề xuất cần review, không là giờ thực tế đã làm. Khi thiếu giờ, ghi chênh lệch và xử lý dự phòng/CR; không tự cắt scope hoặc ghi Done/Pass để vừa ngân sách.
