# Dữ liệu và kịch bản kiểm thử

## Tài khoản và danh mục mẫu

SV-A và SV-B là hai sinh viên khác nhau. NV-A1, NV-A2, NV-A3 là nhân viên hoạt động của phòng ban A; NV-B1 của B. QL-A quản lý A, QL-B quản lý B, QL-AB quản lý cả A và B. Có một tài khoản inactive và một loại inactive để kiểm dữ liệu đã ngừng hoạt động. Mật khẩu test được đặt qua seed môi trường, không ghi mật khẩu thật vào tài liệu.

Phòng ban A và B hoạt động, mỗi bên có QL hoạt động. A1 và A2 thuộc A, B1 thuộc B. Seed tạo mới dùng SLA A1 48 giờ, A2 24 giờ, B1 48 giờ. Khi thử điều kiện SLA không hợp lệ, dùng dataset riêng, không sửa dataset báo cáo đang được đối chiếu.

Mỗi ca chức năng độc lập dùng Ticket mới hoặc reset fixture; record thay đổi thành công cần kèm lịch sử hợp lệ. Dùng đồng hồ test có kiểm soát cho ca biên thời gian, không chờ 30 phút hay thay thời gian hệ thống dùng chung. Ngày trong kịch bản SLA và báo cáo là dữ liệu test, không là lịch dự án.

## Bộ QA RPT

Mọi Ticket Q01 đến Q12 có created_at 2026-10-12 09:00 theo giờ Việt Nam. Chạy báo cáo lọc ngày tạo 2026-10-12 đến 2026-10-12 tại as_of_at 2026-10-14 09:00. Chủ Ticket có thể cùng SV-A; quyền QL mới quyết định phạm vi báo cáo. ended_at ở bảng là số giờ sau created_at. Outcome rỗng khi còn mở. Q02 có snapshot SLA 24 giờ và đã đổi loại từ A2 sang A1 trước mốc báo cáo; due_at vẫn giữ hạn cũ, có lịch sử đổi loại.

| Ticket | PB | Loại | Status | NV | Hạn tháng 10 | Outcome | Giờ kết thúc | Điểm |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Q01 | A | A1 | RECEIVED | Chưa giao | 14-10 09:00 | Rỗng | Rỗng | Rỗng |
| Q02 | A | A1 | RECEIVED | NV-A1 | 13-10 09:00 | Rỗng | Rỗng | Rỗng |
| Q03 | A | A2 | PROCESSING | NV-A1 | 15-10 09:00 | Rỗng | Rỗng | Rỗng |
| Q04 | A | A2 | WAITING_INFO | NV-A2 | 13-10 09:00 | Rỗng | Rỗng | Rỗng |
| Q05 | A | A1 | RESOLVED | NV-A1 | 14-10 09:00 | RESOLVED | 2 | Rỗng |
| Q06 | A | A2 | REJECTED | NV-A2 | 13-10 09:00 | REJECTED | 1 | Rỗng |
| Q07 | A | A1 | CLOSED | NV-A1 | 14-10 09:00 | RESOLVED | 4 | 3 |
| Q08 | A | A2 | CLOSED | NV-A2 | 13-10 09:00 | REJECTED | 3 | 5 |
| Q09 | A | A2 | CLOSED | NV-A1 | 13-10 09:00 | RESOLVED | 6 | Rỗng |
| Q10 | B | B1 | PROCESSING | NV-B1 | 13-10 09:00 | Rỗng | Rỗng | Rỗng |
| Q11 | B | B1 | RESOLVED | NV-B1 | 14-10 09:00 | RESOLVED | 8 | Rỗng |
| Q12 | B | B1 | CLOSED | NV-B1 | 14-10 09:00 | RESOLVED | 10 | 4 |


Q03 được QL gia hạn đến 15-10 09:00 khi còn mở và trước hạn mới, có lý do và lịch sử. Q04 có một câu hỏi OPEN; các CLOSED có closed_at sau ended_at; Q07, Q08, Q12 có một feedback sau closed_at. Nội dung kết quả và lý do được nạp đủ giới hạn. Dataset báo cáo không được dùng để khẳng định toàn bộ luồng ghi đã chạy đúng; luồng ghi được kiểm riêng bằng các TS/E2E.

## Kết quả tính độc lập
| Chỉ số | QL A | QL AB toàn bộ |
| --- | --- | --- |
| Tổng Ticket | 9 | 12 |
| RECEIVED / PROCESSING / WAITING_INFO | 2 / 1 / 1 | 2 / 2 / 1 |
| RESOLVED / REJECTED / CLOSED | 1 / 1 / 3 | 2 / 1 / 4 |
| Quá hạn đang mở | Q02, Q04; tổng 2 | Q02, Q04, Q10; tổng 3 |
| Giờ giải quyết thành công trung bình | (2+4+6)/3 = 4.0; mẫu 3 | (2+4+6+8+10)/5 = 6.0; mẫu 5 |
| Theo loại hiện tại | A1 4; A2 5 | A1 4; A2 5; B1 3 |
| Tải mở theo NV | Chưa giao 1; A1 2; A2 1; A3 0 | Chưa giao 1; A1 2; A2 1; A3 0; B1 1 |
| Điểm trung bình | (3+5)/2 = 4.0; mẫu 2 | (3+5+4)/3 = 4.0; mẫu 3 |
| Tỷ lệ phản hồi | 2/3 = 66.7% | 3/4 = 75.0% |


## Kịch bản kiểm thử đầu cuối

### E2E-01 Tạo yêu cầu hợp lệ

**Workflow:** WF-01

**Thao tác:** SV-A gửi A1, tiêu đề hợp lệ, mô tả 20 ký tự tại clock 12-10 09:00; QL-A mở hàng chờ.

**Expected Result:** Một RECEIVED chưa phân công, đúng SV-A/A/A1, hạn 14-10 09:00; một event tạo và đúng notification.

### E2E-02 Retry sau mất response

**Workflow:** WF-01

**Thao tác:** Ngắt response sau commit tạo; retry năm lần cùng operation_id, cuối cùng mở danh sách.

**Expected Result:** Một Ticket và một mã; không thêm event hay notification; không có hồ sơ một phần.

### E2E-03 Nhận đồng thời

**Workflow:** WF-02

**Thao tác:** Hai NV A gửi claim song song cho cùng Ticket RECEIVED null, cùng version, operation_id riêng.

**Expected Result:** Một người thắng thành PROCESSING; người thua nhận xung đột và không có quyền xử lý.

### E2E-04 Phân công rồi xử lý

**Workflow:** WF-02

**Thao tác:** QL-A giao NV-A1; kiểm RECEIVED; NV-A1 bắt đầu rồi ghi progress; SV-A tải lại.

**Expected Result:** Một người phụ trách; PROCESSING sau bắt đầu; tiến độ công khai đúng nội dung; không đổi hạn.

### E2E-05 Đổi người trong vòng bổ sung

**Workflow:** WF-03

**Thao tác:** NV-A1 hỏi; QL đổi NV-A2; SV dùng version cũ bị chặn, tải lại và trả lời; NV-A2 đọc hội thoại.

**Expected Result:** Một câu hỏi và một answer; WAITING_INFO trở PROCESSING; người mới nhận thông báo; người cũ mất quyền.

### E2E-06 Chuyển phòng ban

**Workflow:** WF-04

**Thao tác:** QL-A chuyển PROCESSING sang B/B1; NV cũ gửi form cũ; QL-B giao NV-B1 rồi NV bắt đầu.

**Expected Result:** Giữ mã, nội dung, lịch sử, created_at, started_at đầu tiên và hạn; nguồn mất quyền; đích xử lý với người mới.

### E2E-07 Giải quyết rồi đóng và đánh giá

**Workflow:** WF-05, WF-06

**Thao tác:** NV hoàn tất với kết quả; SV đọc RESOLVED, đóng rồi cho điểm 5; QL chạy báo cáo.

**Expected Result:** CLOSED outcome RESOLVED, ended_at không đổi; một feedback; RPT-03 tính thành công, RPT-06 tính điểm.

### E2E-08 Từ chối rồi đóng và đánh giá

**Workflow:** WF-05, WF-06

**Thao tác:** NV từ chối có lý do; SV đóng và cho điểm 2; QL đọc báo cáo.

**Expected Result:** CLOSED outcome REJECTED; không vào thời gian giải quyết thành công; vẫn vào phân phối điểm.

### E2E-09 Đúng hạn và vượt hạn

**Workflow:** WF-01 đến WF-05

**Thao tác:** Giữ Ticket mở tại đúng due_at rồi vượt 1 giây; quét mười lần; sau đó hoàn tất và chạy lại.

**Expected Result:** Tại hạn chưa quá; vượt hạn có nhãn và một cảnh báo theo hạn; kết thúc không còn quá hạn đang mở.

### E2E-10 Sai chủ và sai quyền

**Workflow:** Toàn bộ

**Thao tác:** SV-B đọc/trả lời/đóng/đánh giá Ticket SV-A; NV-B xử lý A; QL-A chạy B; thử URL và request trực tiếp.

**Expected Result:** Bị chặn tại máy chủ; không lộ Ticket, không thay dữ liệu, không thêm event nghiệp vụ.

### E2E-11 Rollback và phục hồi job

**Workflow:** Toàn bộ

**Thao tác:** Tiêm lỗi trước commit ghi Ticket; sau đó chạy lại thành công, dừng job notification sau commit rồi khởi động lại.

**Expected Result:** Lượt lỗi không có dữ liệu một phần; lượt thành công tồn tại; job retry tạo đủ notification không trùng.

### E2E-12 Sáu báo cáo cùng định nghĩa

**Workflow:** WF-06 và báo cáo

**Thao tác:** Nạp QA-RPT; chạy cả sáu báo cáo ở QL-A và QL-AB theo ngày và mốc cố định.

**Expected Result:** Khớp bảng tính độc lập; CLOSED không mất outcome; snapshot/filter/quyền thống nhất.

## Kịch bản theo từng tiêu chí nghiệm thu

Mỗi TS dưới đây nối đúng một AC. Dùng tiền điều kiện của FR và fixture sạch, trừ khi thao tác thử nêu rõ dữ liệu hoặc quyền không hợp lệ. Sau mỗi ca ghi, đối chiếu Ticket, dữ liệu con, event, version và notification có liên quan. Expected Result là điều kiện Pass; không ghi kết quả thực tế vào đặc tả.

### FR-IAM-01 Đăng nhập

**TS-IAM-01-01 — AC-IAM-01-01**

Thao tác: Đăng nhập lần lượt bằng SV-A, NV-A1 và QL-A với mật khẩu seed đúng; đối chiếu trang được mở và session mới.

Expected Result: Với tài khoản hoạt động của từng vai trò và mật khẩu đúng, tạo phiên mới và mở đúng khu vực.

**TS-IAM-01-02 — AC-IAM-01-02**

Thao tác: Gửi email rỗng, email abc và mật khẩu rỗng ở các lượt riêng; kiểm lỗi và session.

Expected Result: Email trống hoặc không hợp lệ bị từ chối và không tạo phiên.

**TS-IAM-01-03 — AC-IAM-01-03**

Thao tác: Thử email không tồn tại, mật khẩu sai và tài khoản inactive với mật khẩu đúng; so thông báo.

Expected Result: Sai thông tin hoặc tài khoản vô hiệu hóa đều trả cùng thông báo chung.

**TS-IAM-01-04 — AC-IAM-01-04**

Thao tác: Đăng nhập rồi điều khiển thời gian test tới hơn 30 phút không hoạt động; gửi một yêu cầu đọc và ghi.

Expected Result: Sau 30 phút không hoạt động, yêu cầu nghiệp vụ bị chặn; đăng nhập lại mới được tiếp tục.

**TS-IAM-01-05 — AC-IAM-01-05**

Thao tác: Ghi nhận mã phiên trước đăng nhập; đăng nhập rồi replay mã cũ vào trang cần xác thực.

Expected Result: Mã phiên trước xác thực không được dùng để truy cập phiên sau xác thực.

### FR-IAM-02 Đăng xuất

**TS-IAM-02-01 — AC-IAM-02-01**

Thao tác: Đăng nhập, bấm Đăng xuất và kiểm trang cuối.

Expected Result: Đăng xuất thành công dẫn về trang đăng nhập.

**TS-IAM-02-02 — AC-IAM-02-02**

Thao tác: Replay cookie phiên trước đăng xuất tới endpoint danh sách Ticket.

Expected Result: Gọi lại thao tác nghiệp vụ bằng phiên cũ bị từ chối.

**TS-IAM-02-03 — AC-IAM-02-03**

Thao tác: Đăng xuất rồi bấm Back và tải lại chi tiết Ticket đã xem.

Expected Result: Back hoặc tải lại trang cũ không trả nội dung cần xác thực.

**TS-IAM-02-04 — AC-IAM-02-04**

Thao tác: Đăng nhập lại sau đăng xuất và chạy một thao tác đọc bằng phiên mới.

Expected Result: Đăng nhập lại tạo phiên mới dùng được.

### FR-IAM-03 Kiểm soát quyền truy cập

**TS-IAM-03-01 — AC-IAM-03-01**

Thao tác: Đăng nhập SV-B; sửa ticket_id trên URL và POST sang Ticket của SV-A.

Expected Result: SV không đọc hoặc ghi Ticket của SV khác khi sửa URL hay tham số.

**TS-IAM-03-02 — AC-IAM-03-02**

Thao tác: Đăng nhập NV-A1; mở hàng chờ A rồi thử đọc và xử lý Ticket do NV-A1 phụ trách.

Expected Result: NV thấy hàng chờ RECEIVED chưa phân công của phòng ban mình; chỉ xử lý Ticket đang giao cho mình.

**TS-IAM-03-03 — AC-IAM-03-03**

Thao tác: NV-A2 thử đọc Ticket đã giao NV-A1; NV-B1 thử đọc Ticket A.

Expected Result: NV không đọc Ticket đã giao cho người khác hoặc đã chuyển sang phòng ban khác.

**TS-IAM-03-04 — AC-IAM-03-04**

Thao tác: QL-A chạy danh sách và báo cáo A, rồi gửi department_id B trực tiếp.

Expected Result: QL chỉ tra cứu, điều phối và báo cáo trên các phòng ban được quản lý.

**TS-IAM-03-05 — AC-IAM-03-05**

Thao tác: SV-A gửi payload tự khai role QL, student_id SV-B hoặc department_id B vào thao tác ghi.

Expected Result: Thay role, student_id hay department_id từ trình duyệt không cấp thêm quyền.

**TS-IAM-03-06 — AC-IAM-03-06**

Thao tác: Mở form ở NV-A1; QL-A đổi assignee sang NV-A2; dùng form cũ gửi hoàn tất.

Expected Result: Sau đổi người hoặc chuyển phòng ban, màn hình đang mở phải được kiểm quyền lại khi gửi.

### FR-STU-01 Xem danh sách yêu cầu của tôi

**TS-STU-01-01 — AC-STU-01-01**

Thao tác: SV-A xem danh sách rồi thêm tham số student_id SV-B; đối chiếu chủ mọi dòng.

Expected Result: Chỉ hiển thị Ticket của SV trong phiên, kể cả khi sửa tham số student_id.

**TS-STU-01-02 — AC-STU-01-02**

Thao tác: Tạo Ticket có mã/tiêu đề khác nhau; tìm một từ khóa và lọc PROCESSING đồng thời.

Expected Result: Kết hợp từ khóa và trạng thái trả đúng tập kết quả.

**TS-STU-01-03 — AC-STU-01-03**

Thao tác: Nạp 41 Ticket của SV-A và 5 của SV-B; kiểm total, total_pages và các dòng.

Expected Result: Tổng trang và danh sách dùng cùng bộ lọc và quyền.

**TS-STU-01-04 — AC-STU-01-04**

Thao tác: Ở trang 2 với từ khóa và trạng thái, đổi sang trang 3 và tải lại; kiểm bộ lọc và thứ tự.

Expected Result: Đổi trang giữ từ khóa và trạng thái; thứ tự ổn định.

**TS-STU-01-05 — AC-STU-01-05**

Thao tác: Dùng tài khoản SV chưa có Ticket; mở danh sách.

Expected Result: Không có Ticket hiển thị Chưa có yêu cầu và liên kết Tạo Ticket.

### FR-STU-02 Sinh viên gửi yêu cầu hỗ trợ

**TS-STU-02-01 — AC-STU-02-01**

Thao tác: SV-A chọn A1, tiêu đề Hỗ trợ đăng ký học phần, mô tả 20 ký tự; gửi một lần.

Expected Result: Dữ liệu hợp lệ tạo đúng một Ticket, RECEIVED, assignee_id rỗng, mã duy nhất.

**TS-STU-02-02 — AC-STU-02-02**

Thao tác: Gửi dữ liệu hợp lệ kèm student_id SV-B và department_id B giả; kiểm giá trị thực lưu, created_at và due_at.

Expected Result: student_id lấy từ phiên, department_id lấy từ danh mục, created_at và due_at lấy từ máy chủ.

**TS-STU-02-03 — AC-STU-02-03**

Thao tác: Gửi mô tả rỗng rồi 20 dấu cách trong hai lượt; kiểm số Ticket trước/sau.

Expected Result: Mô tả trống hoặc chỉ khoảng trắng không tạo Ticket.

**TS-STU-02-04 — AC-STU-02-04**

Thao tác: Thử tiêu đề 4, 5, 150, 151 ký tự và mô tả 9, 10, 3000, 3001 ký tự ở các lượt độc lập.

Expected Result: Tiêu đề 4 hoặc 151 ký tự, mô tả 9 hoặc 3001 ký tự bị từ chối; giá trị biên hợp lệ được nhận.

**TS-STU-02-05 — AC-STU-02-05**

Thao tác: Gửi năm request đồng thời có cùng operation_id và payload; đếm Ticket và event.

Expected Result: Bấm Gửi nhiều lần cùng operation_id chỉ có một Ticket, một sự kiện tạo và cùng mã xác nhận.

**TS-STU-02-06 — AC-STU-02-06**

Thao tác: Cho máy chủ commit nhưng ngắt response; retry cùng operation_id và payload.

Expected Result: Retry sau mất kết nối trả Ticket đã tạo, không tạo thêm.

**TS-STU-02-07 — AC-STU-02-07**

Thao tác: Tiêm lỗi sau insert Ticket nhưng trước insert event hoặc idempotency result; kiểm rollback mọi bảng.

Expected Result: Một phần ghi thất bại không để Ticket, lịch sử hoặc thông báo mồ côi.

### FR-STU-03 Xem chi tiết và tiến độ yêu cầu của tôi

**TS-STU-03-01 — AC-STU-03-01**

Thao tác: Mở chi tiết rồi cập nhật Ticket bằng actor hợp lệ khác; tải lại và so dữ liệu hiện hành với danh sách.

Expected Result: Tải lại hiển thị cùng trạng thái, phòng ban, người phụ trách và hạn với dữ liệu máy chủ.

**TS-STU-03-02 — AC-STU-03-02**

Thao tác: Mở một Ticket chưa phân công và một Ticket mở đã quá hạn; kiểm nhãn và status.

Expected Result: Chưa phân công hiển thị Chưa phân công; quá hạn là nhãn riêng, không thay trạng thái.

**TS-STU-03-03 — AC-STU-03-03**

Thao tác: NV tạo câu hỏi bổ sung; SV mở chi tiết và liên kết trả lời đúng câu hỏi.

Expected Result: WAITING_INFO hiển thị yêu cầu bổ sung đang mở; chỉ cho liên kết trả lời tương ứng.

**TS-STU-03-04 — AC-STU-03-04**

Thao tác: Mở lần lượt Ticket RESOLVED và REJECTED của mình; đọc kết quả và nút đóng.

Expected Result: RESOLVED hoặc REJECTED hiển thị nội dung kết quả và nút xác nhận đóng.

**TS-STU-03-05 — AC-STU-03-05**

Thao tác: Đóng Ticket chưa đánh giá rồi tải lại; kiểm outcome, nội dung kết quả và liên kết đánh giá.

Expected Result: CLOSED giữ nội dung kết quả và cho đánh giá nếu chưa có đánh giá.

**TS-STU-03-06 — AC-STU-03-06**

Thao tác: SV-B mở URL của Ticket SV-A; kiểm response và giao diện không có dữ liệu Ticket.

Expected Result: Đường dẫn của SV khác không lộ tiêu đề, nội dung, hội thoại hoặc lịch sử.

### FR-TKT-01 Xem hàng chờ và tìm kiếm Ticket

**TS-TKT-01-01 — AC-TKT-01-01**

Thao tác: NV-A1 mở Hàng chờ với dữ liệu RECEIVED null, RECEIVED đã giao và PROCESSING ở A/B.

Expected Result: NV ở Hàng chờ chỉ thấy RECEIVED, assignee_id rỗng, đúng phòng ban mình.

**TS-TKT-01-02 — AC-TKT-01-02**

Thao tác: NV-A1 mở Công việc của tôi; kiểm Ticket của mình ở cả trạng thái mở và đã kết thúc.

Expected Result: NV ở Công việc của tôi thấy Ticket đang được giao cho mình, kể cả đã có kết quả.

**TS-TKT-01-03 — AC-TKT-01-03**

Thao tác: QL-A kết hợp lọc status, category và assignment; so tập tính độc lập.

Expected Result: QL thấy Ticket thuộc tập phòng ban được quản lý và đúng bộ lọc.

**TS-TKT-01-04 — AC-TKT-01-04**

Thao tác: Nhận hoặc phân công một Ticket từ hàng chờ; tải lại tập chưa phân công và tập tất cả.

Expected Result: Nhận hoặc phân công thành công làm Ticket rời tập chưa phân công khi tải lại.

**TS-TKT-01-05 — AC-TKT-01-05**

Thao tác: Nạp hơn 20 Ticket; kiểm total, thứ tự, phân trang và từ chối bộ lọc B ở QL-A.

Expected Result: Tổng số dòng, trang và kết quả cùng tuân thủ quyền và bộ lọc.

### FR-TKT-02 Điều chỉnh loại vấn đề của Ticket

**TS-TKT-02-01 — AC-TKT-02-01**

Thao tác: QL-A đổi A1 sang A2 cho Ticket PROCESSING, lý do 20 ký tự; so các trường không đổi.

Expected Result: Loại hợp lệ được đổi, giữ status, department_id, assignee_id và due_at.

**TS-TKT-02-02 — AC-TKT-02-02**

Thao tác: Kiểm event CATEGORY_CHANGED sau một lần đổi; đối chiếu actor, cũ, mới và reason.

Expected Result: Ghi một sự kiện CATEGORY_CHANGED có loại cũ, mới, lý do và actor.

**TS-TKT-02-03 — AC-TKT-02-03**

Thao tác: Thử loại B1 và loại A inactive bằng request trực tiếp.

Expected Result: Loại khác phòng ban hoặc ngừng hoạt động bị từ chối.

**TS-TKT-02-04 — AC-TKT-02-04**

Thao tác: Thử reason rỗng, 9, 10, 500 và 501 ký tự ở các lượt có reset dữ liệu.

Expected Result: Lý do trống, dưới 10 hoặc trên 500 ký tự không lưu.

**TS-TKT-02-05 — AC-TKT-02-05**

Thao tác: Hai QL dùng cùng version đổi sang hai loại khác nhau; gửi lượt thứ hai sau lượt đầu.

Expected Result: Version cũ không ghi đè phân loại vừa được cập nhật.

### FR-TKT-03 Xem chi tiết Ticket nội bộ

**TS-TKT-03-01 — AC-TKT-03-01**

Thao tác: NV-A1 mở Ticket của mình và Ticket RECEIVED chưa giao A.

Expected Result: NV xem được Ticket đang giao cho mình hoặc Ticket RECEIVED chưa phân công của phòng ban mình.

**TS-TKT-03-02 — AC-TKT-03-02**

Thao tác: QL-A mở chi tiết A rồi thử B.

Expected Result: QL xem được Ticket thuộc phòng ban quản lý.

**TS-TKT-03-03 — AC-TKT-03-03**

Thao tác: QL hoặc NV hợp lệ mở Ticket và so version/status/due_at/assignee/outcome với database.

Expected Result: Hiển thị đúng version, trạng thái, hạn, người phụ trách và kết quả hiện hành.

**TS-TKT-03-04 — AC-TKT-03-04**

Thao tác: Mở các trạng thái dưới hai vai trò NV và QL; kiểm các nút và gọi trực tiếp thao tác bị ẩn.

Expected Result: NV chỉ thấy nút xử lý trên Ticket của mình; QL chỉ thấy điều phối theo trạng thái.

**TS-TKT-03-05 — AC-TKT-03-05**

Thao tác: Đọc chi tiết Ticket chưa giao ba lần; so assignee và status trước/sau.

Expected Result: Màn hình đọc không thay trạng thái hoặc tự nhận Ticket.

### FR-TKT-04 Xem lịch sử nghiệp vụ Ticket

**TS-TKT-04-01 — AC-TKT-04-01**

Thao tác: Tạo Ticket rồi lần lượt phân công, bắt đầu, đổi hạn và hoàn tất; đối chiếu event với từng thao tác.

Expected Result: Ticket mới có một TICKET_CREATED; mỗi thay đổi thành công có sự kiện tương ứng.

**TS-TKT-04-02 — AC-TKT-04-02**

Thao tác: Seed hai event có cùng occurred_at nhưng event_id khác; mở lịch sử.

Expected Result: Thứ tự ổn định khi hai sự kiện cùng thời điểm.

**TS-TKT-04-03 — AC-TKT-04-03**

Thao tác: Retry một thao tác ghi cùng operation_id ba lần; đếm event cùng hành động.

Expected Result: Retry cùng thao tác không thêm sự kiện nghiệp vụ.

**TS-TKT-04-04 — AC-TKT-04-04**

Thao tác: Kiểm UI và thử yêu cầu sửa/xóa event bằng endpoint nghiệp vụ.

Expected Result: Không có thao tác sửa hoặc xóa lịch sử trên giao diện hay endpoint nghiệp vụ.

**TS-TKT-04-05 — AC-TKT-04-05**

Thao tác: Đổi người khỏi NV-A1, NV-A1 truy cập lịch sử; SV mở lịch sử hợp lệ và kiểm trường bí mật.

Expected Result: NV mất quyền Ticket cũng mất quyền lịch sử; SV không thấy email, session hay dữ liệu bí mật của actor.

### FR-OPS-01 Nhận Ticket từ hàng chờ

**TS-OPS-01-01 — AC-OPS-01-01**

Thao tác: NV-A1 nhận Ticket RECEIVED null của A; kiểm assignee, status và started_at.

Expected Result: Nhận hợp lệ gán đúng NV trong phiên, chuyển PROCESSING và ghi started_at.

**TS-OPS-01-02 — AC-OPS-01-02**

Thao tác: Gửi claim đồng thời từ NV-A1 và NV-A2 với cùng version và operation_id riêng.

Expected Result: Hai NV nhận đồng thời: đúng một người thành công, một người nhận xung đột.

**TS-OPS-01-03 — AC-OPS-01-03**

Thao tác: NV-A1 claim Ticket đã giao NV-A2 rồi claim Ticket chưa giao B.

Expected Result: NV không nhận Ticket đã phân công cho người khác hoặc ở phòng ban khác.

**TS-OPS-01-04 — AC-OPS-01-04**

Thao tác: Retry claim thắng với cùng operation_id sau mất response; so started_at và event.

Expected Result: Retry của người đã nhận trả kết quả cũ; không thêm lịch sử hay thay started_at.

**TS-OPS-01-05 — AC-OPS-01-05**

Thao tác: Sau claim đồng thời, cả người thắng và thua thử đọc chi tiết và gửi progress.

Expected Result: NV thắng có quyền xử lý, NV còn lại không có quyền đọc nội dung đã được phân công.

### FR-OPS-02 Bắt đầu xử lý Ticket được phân công

**TS-OPS-02-01 — AC-OPS-02-01**

Thao tác: QL-A giao RECEIVED cho NV-A1; NV-A1 bấm Bắt đầu.

Expected Result: NV được giao bắt đầu hợp lệ, status thành PROCESSING.

**TS-OPS-02-02 — AC-OPS-02-02**

Thao tác: Bắt đầu Ticket chưa có started_at; so mốc máy chủ.

Expected Result: Ticket chưa có started_at được ghi thời điểm máy chủ.

**TS-OPS-02-03 — AC-OPS-02-03**

Thao tác: Chuyển Ticket từng PROCESSING A sang B, giao NV-B1 và bắt đầu lại; so started_at cũ.

Expected Result: Ticket đã có started_at giữ mốc đầu tiên sau chuyển phòng ban.

**TS-OPS-02-04 — AC-OPS-02-04**

Thao tác: QL-A và NV-A2 gửi bắt đầu Ticket giao NV-A1.

Expected Result: QL hoặc NV khác không bắt đầu thay người được giao.

**TS-OPS-02-05 — AC-OPS-02-05**

Thao tác: Retry Bắt đầu bằng cùng operation_id; kiểm một event và một version increment.

Expected Result: Gửi lặp cùng operation_id chỉ tạo một sự kiện và một lần chuyển trạng thái.

### FR-OPS-03 Ghi cập nhật tiến độ xử lý

**TS-OPS-03-01 — AC-OPS-03-01**

Thao tác: NV-A1 ghi nội dung 20 ký tự trên Ticket PROCESSING của mình; so status và hạn.

Expected Result: Nội dung hợp lệ lưu đúng Ticket và actor, giữ PROCESSING và hạn.

**TS-OPS-03-02 — AC-OPS-03-02**

Thao tác: Sau cập nhật, SV-A tải lại chi tiết; đối chiếu đúng nội dung.

Expected Result: SV thấy nội dung khi tải lại chi tiết.

**TS-OPS-03-03 — AC-OPS-03-03**

Thao tác: Gửi lại progress cùng operation_id; đếm progress, event và notification.

Expected Result: Retry cùng thao tác không thêm cập nhật hoặc thông báo.

**TS-OPS-03-04 — AC-OPS-03-04**

Thao tác: QL-A đổi người sang NV-A2 trước khi NV-A1 gửi form progress cũ.

Expected Result: Người phụ trách cũ không ghi sau đổi người.

**TS-OPS-03-05 — AC-OPS-03-05**

Thao tác: Ghi chuỗi `<script>alert(1)</script>` và mở dưới NV, SV; kiểm không chạy script.

Expected Result: Nội dung có ký tự HTML hiển thị như văn bản, không thực thi.

### FR-OPS-04 Hoàn tất xử lý Ticket

**TS-OPS-04-01 — AC-OPS-04-01**

Thao tác: NV-A1 hoàn tất PROCESSING không câu hỏi mở với resolution 20 ký tự; kiểm outcome, ended_at.

Expected Result: Kết quả hợp lệ chuyển PROCESSING sang RESOLVED, outcome RESOLVED, ended_at theo máy chủ.

**TS-OPS-04-02 — AC-OPS-04-02**

Thao tác: Ngay sau hoàn tất, kiểm closed_at rỗng; sau đó SV dùng thao tác đóng.

Expected Result: Không tạo closed_at; SV phải xác nhận đóng ở FR-FDB-01.

**TS-OPS-04-03 — AC-OPS-04-03**

Thao tác: Thử resolution rỗng, 9, 10, 5000, 5001 ký tự trên Ticket được reset từng lượt.

Expected Result: Thiếu hoặc dưới 10 ký tự không lưu kết quả hay đổi trạng thái.

**TS-OPS-04-04 — AC-OPS-04-04**

Thao tác: Thử hoàn tất WAITING_INFO và gửi từ NV-A2 trên Ticket NV-A1.

Expected Result: WAITING_INFO và NV khác không hoàn tất.

**TS-OPS-04-05 — AC-OPS-04-05**

Thao tác: Cho hoàn tất commit, ngắt response rồi retry cùng mã; kiểm event và thông báo.

Expected Result: Retry cùng mã chỉ có một sự kiện hoàn tất và một thông báo cho SV.

**TS-OPS-04-06 — AC-OPS-04-06**

Thao tác: Sau RESOLVED, gọi trực tiếp phân công, progress, bổ sung và đổi hạn.

Expected Result: Sau hoàn tất, các thao tác phân công, tiến độ, bổ sung và đổi hạn đều bị chặn.

### FR-OPS-05 Từ chối xử lý Ticket

**TS-OPS-05-01 — AC-OPS-05-01**

Thao tác: NV-A1 từ chối PROCESSING bằng lý do 20 ký tự; SV đọc kết quả.

Expected Result: Từ chối hợp lệ tạo REJECTED, outcome REJECTED, ended_at và lý do đọc được bởi SV.

**TS-OPS-05-02 — AC-OPS-05-02**

Thao tác: Sau từ chối, kiểm resolution và closed_at rỗng, rejection_reason có giá trị.

Expected Result: Không ghi resolution hoặc closed_at thay cho từ chối.

**TS-OPS-05-03 — AC-OPS-05-03**

Thao tác: Thử lý do rỗng, 9, 10, 1000, 1001 ký tự ở các lượt reset.

Expected Result: Lý do trống, 9 hoặc 1001 ký tự bị từ chối.

**TS-OPS-05-04 — AC-OPS-05-04**

Thao tác: Gửi từ chối Ticket WAITING_INFO còn câu hỏi OPEN.

Expected Result: WAITING_INFO không được chuyển trực tiếp REJECTED.

**TS-OPS-05-05 — AC-OPS-05-05**

Thao tác: Retry từ chối cùng operation_id; kiểm ended_at và số event/notification.

Expected Result: Retry không thêm sự kiện từ chối hoặc thông báo.

**TS-OPS-05-06 — AC-OPS-05-06**

Thao tác: SV đóng Ticket REJECTED rồi gửi điểm 2; kiểm outcome và feedback.

Expected Result: SV có thể xác nhận đóng và đánh giá Ticket bị từ chối theo WF-06.

### FR-ASG-01 Phân công người phụ trách Ticket

**TS-ASG-01-01 — AC-ASG-01-01**

Thao tác: QL-A giao RECEIVED null cho NV-A1; so status và due_at trước/sau.

Expected Result: Phân công hợp lệ điền đúng assignee_id, giữ RECEIVED và due_at.

**TS-ASG-01-02 — AC-ASG-01-02**

Thao tác: NV-A1 mở Công việc của tôi và Thông báo sau khi được giao.

Expected Result: NV được giao thấy Ticket trong Công việc của tôi và nhận thông báo.

**TS-ASG-01-03 — AC-ASG-01-03**

Thao tác: QL-A gửi phân công Ticket B trực tiếp.

Expected Result: QL không phân công Ticket ngoài phòng ban quản lý.

**TS-ASG-01-04 — AC-ASG-01-04**

Thao tác: Thử NV inactive A và NV-B1 bằng payload trực tiếp.

Expected Result: Không chọn được NV ngừng hoạt động hoặc khác phòng ban; máy chủ kiểm lại.

**TS-ASG-01-05 — AC-ASG-01-05**

Thao tác: Chạy claim NV-A2 đồng thời phân công NV-A1 trên cùng version.

Expected Result: Nhận việc và phân công đồng thời vẫn có tối đa một người phụ trách.

**TS-ASG-01-06 — AC-ASG-01-06**

Thao tác: Retry phân công cùng mã; đếm event và notification mỗi recipient.

Expected Result: Retry không thêm lịch sử hoặc thông báo.

### FR-ASG-02 Đổi người phụ trách Ticket

**TS-ASG-02-01 — AC-ASG-02-01**

Thao tác: Đổi NV-A1 sang NV-A2 lần lượt ở RECEIVED, PROCESSING, WAITING_INFO; so các trường còn lại.

Expected Result: Đổi hợp lệ giữ status, department_id, due_at, started_at và câu hỏi đang mở.

**TS-ASG-02-02 — AC-ASG-02-02**

Thao tác: Đọc event ASSIGNEE_CHANGED và so người cũ mới, reason, actor.

Expected Result: Lịch sử ghi người cũ, mới và lý do.

**TS-ASG-02-03 — AC-ASG-02-03**

Thao tác: Sau đổi, NV cũ và mới mở chi tiết và gửi thao tác theo status.

Expected Result: Người cũ không còn đọc hay ghi Ticket; người mới được xử lý theo trạng thái.

**TS-ASG-02-04 — AC-ASG-02-04**

Thao tác: Đổi người khi câu hỏi OPEN; SV tải lại, trả lời; kiểm notification recipient NV mới.

Expected Result: WAITING_INFO giữ câu hỏi đang mở; câu trả lời sau đó được thông báo cho người mới.

**TS-ASG-02-05 — AC-ASG-02-05**

Thao tác: Thử đổi assignee sang NV-B1 hoặc tài khoản inactive.

Expected Result: Không đổi sang NV khác phòng ban.

**TS-ASG-02-06 — AC-ASG-02-06**

Thao tác: Retry cùng mã sau đổi; kiểm thông báo không lặp cho người cũ/mới/SV.

Expected Result: Retry cùng thao tác không gửi lặp thông báo.

### FR-ASG-03 Chuyển Ticket sang phòng ban khác

**TS-ASG-03-01 — AC-ASG-03-01**

Thao tác: QL-A chuyển Ticket từng bắt đầu sang B/B1; so mã, chủ, mô tả, created_at, started_at, due_at.

Expected Result: Chuyển hợp lệ giữ ticket_id, mã, student_id, nội dung gốc, created_at, started_at và due_at.

**TS-ASG-03-02 — AC-ASG-03-02**

Thao tác: Sau chuyển, kiểm department/category đích, assignee null và RECEIVED cùng giao dịch.

Expected Result: Phòng ban và loại đổi đồng thời, assignee_id rỗng, status RECEIVED.

**TS-ASG-03-03 — AC-ASG-03-03**

Thao tác: Đối chiếu event chuyển chứa nguồn/đích, loại trước/sau, NV cũ và reason.

Expected Result: Lịch sử giữ phòng ban, loại, người cũ và lý do.

**TS-ASG-03-04 — AC-ASG-03-04**

Thao tác: NV cũ và QL-A thử đọc sau chuyển; QL-B mở hàng chờ B.

Expected Result: NV cũ và QL chỉ quản lý nguồn mất quyền Ticket sau chuyển; QL đích và hàng chờ đích thấy Ticket.

**TS-ASG-03-05 — AC-ASG-03-05**

Thao tác: QL-A chỉ quản lý A chuyển sang B; kiểm response xác nhận không kèm nội dung ngoài quyền và không đọc lại được.

Expected Result: QL nguồn được chuyển tới đích hoạt động dù không quản lý đích; không được đọc Ticket sau chuyển nếu không có quyền đích.

**TS-ASG-03-06 — AC-ASG-03-06**

Thao tác: Thử chuyển WAITING_INFO còn câu hỏi OPEN; so câu hỏi và Ticket trước/sau.

Expected Result: WAITING_INFO bị chặn, không làm mất nội dung trao đổi.

**TS-ASG-03-07 — AC-ASG-03-07**

Thao tác: Retry chuyển cùng operation_id; kiểm event và notification không tăng.

Expected Result: Retry không chuyển lại hoặc nhân đôi thông báo.

### FR-COM-01 Yêu cầu sinh viên bổ sung thông tin

**TS-COM-01-01 — AC-COM-01-01**

Thao tác: NV-A1 gửi câu hỏi 20 ký tự trên PROCESSING; kiểm một OPEN và WAITING_INFO.

Expected Result: Gửi hợp lệ tạo đúng một câu hỏi OPEN và chuyển PROCESSING sang WAITING_INFO.

**TS-COM-01-02 — AC-COM-01-02**

Thao tác: So assignee, department, due_at trước/sau yêu cầu bổ sung; đẩy thời gian qua hạn.

Expected Result: Giữ người phụ trách, phòng ban và due_at; SLA tiếp tục tính thời gian.

**TS-COM-01-03 — AC-COM-01-03**

Thao tác: SV-A mở chi tiết và thông báo, kiểm câu hỏi đúng Ticket.

Expected Result: SV thấy câu hỏi và nút Trả lời.

**TS-COM-01-04 — AC-COM-01-04**

Thao tác: Khi WAITING_INFO, gọi trực tiếp hỏi lần hai, progress, hoàn tất và từ chối.

Expected Result: Khi WAITING_INFO không thể gửi câu hỏi thứ hai, ghi tiến độ, hoàn tất hoặc từ chối.

**TS-COM-01-05 — AC-COM-01-05**

Thao tác: Gửi câu hỏi năm lần với cùng mã; đếm request, event và notification.

Expected Result: Retry cùng thao tác chỉ có một câu hỏi, sự kiện và thông báo.

### FR-COM-02 Sinh viên trả lời yêu cầu bổ sung

**TS-COM-02-01 — AC-COM-02-01**

Thao tác: SV-A trả lời câu hỏi OPEN bằng nội dung 20 ký tự; kiểm answer, ANSWERED và PROCESSING.

Expected Result: Câu trả lời hợp lệ tạo một answer, câu hỏi ANSWERED, status PROCESSING.

**TS-COM-02-02 — AC-COM-02-02**

Thao tác: So mã, chủ, assignee, department, due_at trước/sau trả lời.

Expected Result: Giữ ticket_id, student_id, assignee_id, department_id và due_at.

**TS-COM-02-03 — AC-COM-02-03**

Thao tác: Thử câu trả lời rỗng, chỉ khoảng trắng, 1, 2000, 2001 ký tự.

Expected Result: Nội dung trống, chỉ khoảng trắng hoặc 2001 ký tự bị từ chối.

**TS-COM-02-04 — AC-COM-02-04**

Thao tác: Gửi lại answer với cùng operation_id; đếm answer và event.

Expected Result: Gửi lặp cùng operation_id không thêm câu trả lời hay sự kiện.

**TS-COM-02-05 — AC-COM-02-05**

Thao tác: Đổi NV phụ trách, SV tải lại rồi trả lời; đối chiếu người nhận thông báo.

Expected Result: Sau đổi người, câu trả lời được thông báo cho NV mới.

**TS-COM-02-06 — AC-COM-02-06**

Thao tác: Hai tab SV gửi hai operation_id cho cùng câu hỏi; sau đó NV tạo câu hỏi vòng hai.

Expected Result: Một câu hỏi chỉ có một câu trả lời hoàn tất; muốn bổ sung tiếp NV phải tạo câu hỏi mới khi PROCESSING.

### FR-COM-03 Xem hội thoại bổ sung của Ticket

**TS-COM-03-01 — AC-COM-03-01**

Thao tác: Tạo ba vòng hỏi trả lời trên cùng Ticket; mở hội thoại và đối chiếu từng cặp.

Expected Result: Nhiều vòng bổ sung được hiển thị đúng thứ tự và đúng quan hệ câu hỏi trả lời.

**TS-COM-03-02 — AC-COM-03-02**

Thao tác: Tạo câu hỏi OPEN cuối cùng; kiểm nhãn và liên kết trả lời dưới SV, NV, QL.

Expected Result: Câu hỏi OPEN hiển thị Chờ trả lời; chỉ SV chủ Ticket có liên kết trả lời.

**TS-COM-03-03 — AC-COM-03-03**

Thao tác: QL và NV gửi trực tiếp answer thay SV.

Expected Result: QL và NV được đọc theo quyền nhưng không trả lời thay SV.

**TS-COM-03-04 — AC-COM-03-04**

Thao tác: Đổi người rồi chuyển phòng ban sau khi câu hỏi đã trả lời; đọc lại các vòng ở actor có quyền.

Expected Result: Nội dung không bị sửa hoặc mất sau phân công lại hoặc chuyển phòng ban.

**TS-COM-03-05 — AC-COM-03-05**

Thao tác: Dùng chuỗi HTML trong câu hỏi và answer; đọc hội thoại dưới cả ba vai trò.

Expected Result: HTML trong câu hỏi hoặc câu trả lời không thực thi.

### FR-NOT-01 Tạo thông báo trong ứng dụng theo sự kiện

**TS-NOT-01-01 — AC-NOT-01-01**

Thao tác: Chạy từng sự kiện trong bảng định tuyến với các tài khoản hoạt động; so tập recipient thực tế.

Expected Result: Mỗi sự kiện thuộc bảng định tuyến tạo đúng tập người nhận được định nghĩa.

**TS-NOT-01-02 — AC-NOT-01-02**

Thao tác: Xử lý cùng event hai lần và kiểm unique(event_id, recipient_id).

Expected Result: Retry cùng event_id và recipient_id không tạo thông báo thứ hai.

**TS-NOT-01-03 — AC-NOT-01-03**

Thao tác: Tiêm lỗi giao dịch Ticket trước commit rồi chạy bộ xử lý notification.

Expected Result: Giao dịch Ticket rollback không tạo thông báo.

**TS-NOT-01-04 — AC-NOT-01-04**

Thao tác: Tiêm lỗi tạo notification cho recipient thứ hai; retry event và đếm từng recipient.

Expected Result: Một recipient thất bại không làm mất các recipient còn lại; lần retry chỉ bù phần thiếu.

**TS-NOT-01-05 — AC-NOT-01-05**

Thao tác: Commit một event khi job hoạt động; đo đến lúc thông báo xuất hiện, giới hạn 60 giây.

Expected Result: Thông báo hiển thị trong 60 giây khi bộ xử lý hoạt động.

**TS-NOT-01-06 — AC-NOT-01-06**

Thao tác: Kiểm payload mọi thông báo tạo, không có description, answer, mật khẩu hoặc cookie.

Expected Result: Nội dung chỉ có mã Ticket, tên hành động và thời điểm; không có mô tả nhạy cảm.

### FR-NOT-02 Xem danh sách thông báo của tôi

**TS-NOT-02-01 — AC-NOT-02-01**

Thao tác: NV-A1 xem danh sách rồi sửa recipient_id thành NV-A2.

Expected Result: Chỉ nhận được notification của tài khoản trong phiên.

**TS-NOT-02-02 — AC-NOT-02-02**

Thao tác: Seed 3 unread, 2 read cho tài khoản; chọn ALL và UNREAD, đối chiếu số đếm.

Expected Result: UNREAD lọc read_at rỗng; số chưa đọc khớp cùng tài khoản.

**TS-NOT-02-03 — AC-NOT-02-03**

Thao tác: NV cũ mở thông báo phân công sau khi QL đổi người hoặc chuyển phòng ban.

Expected Result: Mở Ticket kiểm quyền hiện tại, không dùng quyền lúc gửi thông báo.

**TS-NOT-02-04 — AC-NOT-02-04**

Thao tác: Tải danh sách thông báo mà không mở dòng; so read_at trước/sau.

Expected Result: Xem danh sách chưa tự đánh dấu tất cả đã đọc.

**TS-NOT-02-05 — AC-NOT-02-05**

Thao tác: Mở Thông báo bằng tài khoản chưa có notification.

Expected Result: Không có thông báo hiển thị Bạn chưa có thông báo.

### FR-NOT-03 Đánh dấu một thông báo đã đọc

**TS-NOT-03-01 — AC-NOT-03-01**

Thao tác: Đánh dấu một unread của mình; kiểm read_at theo máy chủ.

Expected Result: Thông báo chưa đọc của mình được ghi read_at bằng thời điểm máy chủ.

**TS-NOT-03-02 — AC-NOT-03-02**

Thao tác: Lặp đánh dấu cùng notification sau 5 giây; so read_at đầu.

Expected Result: Đánh dấu lần hai không thay read_at đầu tiên.

**TS-NOT-03-03 — AC-NOT-03-03**

Thao tác: Gửi notification_id thuộc người khác.

Expected Result: Không đánh dấu được thông báo của tài khoản khác.

**TS-NOT-03-04 — AC-NOT-03-04**

Thao tác: Đánh dấu cùng unread từ hai tab; kiểm số chưa đọc giảm một, không âm.

Expected Result: Số chưa đọc giảm đúng một và không âm.

**TS-NOT-03-05 — AC-NOT-03-05**

Thao tác: Mở thông báo cũ của mình khi không còn quyền Ticket; kiểm read_at và việc chặn nội dung.

Expected Result: Thông báo được đánh dấu đã đọc dù liên kết Ticket hiện không còn quyền; việc đọc Ticket vẫn bị chặn.

### FR-SLA-01 Tính hạn xử lý khi tạo Ticket

**TS-SLA-01-01 — AC-SLA-01-01**

Thao tác: Tạo Ticket A1 có SLA 48 giờ tại clock test 2026-10-12 09:00 giờ Việt Nam.

Expected Result: SLA 48 giờ và created_at 2026-10-12 09:00 giờ Việt Nam cho due_at 2026-10-14 09:00.

**TS-SLA-01-02 — AC-SLA-01-02**

Thao tác: Tiêm lỗi khi lưu due_at trong giao dịch tạo; kiểm không có Ticket thiếu snapshot hoặc hạn.

Expected Result: Hạn và snapshot được lưu trong cùng giao dịch tạo Ticket.

**TS-SLA-01-03 — AC-SLA-01-03**

Thao tác: Tạo Ticket thứ Sáu 16:00 SLA 48; đưa WAITING_INFO và kiểm hạn vẫn Chủ nhật 16:00.

Expected Result: Cuối tuần và WAITING_INFO không dừng đồng hồ SLA.

**TS-SLA-01-04 — AC-SLA-01-04**

Thao tác: Đổi loại, đổi người và chuyển phòng ban; so due_at với hạn trước.

Expected Result: Đổi loại, phân công lại hoặc chuyển phòng ban không tự tính lại due_at.

**TS-SLA-01-05 — AC-SLA-01-05**

Thao tác: Thử seed SLA null, 0, 169; gửi tạo Ticket và kiểm không có dữ liệu một phần.

Expected Result: SLA cấu hình không hợp lệ ngăn tạo Ticket, không để dữ liệu thiếu hạn.

### FR-SLA-02 Điều chỉnh hạn xử lý Ticket

**TS-SLA-02-01 — AC-SLA-02-01**

Thao tác: QL-A đặt hạn tương lai kèm lý do hợp lệ; kiểm snapshot và status không đổi.

Expected Result: Đổi hợp lệ lưu đúng thời điểm và lý do, giữ status và sla_hours_snapshot.

**TS-SLA-02-02 — AC-SLA-02-02**

Thao tác: Thử hạn trước created_at và trước clock máy chủ trong các lượt riêng.

Expected Result: Hạn trước created_at hoặc trước thời điểm máy chủ bị từ chối.

**TS-SLA-02-03 — AC-SLA-02-03**

Thao tác: Giữ clock test cố định; đặt hạn đúng clock và đọc ngay is_overdue.

Expected Result: Mốc đúng thời điểm máy chủ được chấp nhận; tại đúng hạn chưa quá hạn.

**TS-SLA-02-04 — AC-SLA-02-04**

Thao tác: Thử đổi hạn Ticket RESOLVED và Ticket B bằng QL-A.

Expected Result: Ticket kết thúc hoặc ngoài phạm vi bị chặn.

**TS-SLA-02-05 — AC-SLA-02-05**

Thao tác: Đọc DUE_DATE_CHANGED rồi chạy lại báo cáo quá hạn ở cùng mốc phù hợp.

Expected Result: Lịch sử ghi hạn cũ và mới; báo cáo sau tải lại dùng hạn mới.

**TS-SLA-02-06 — AC-SLA-02-06**

Thao tác: Gia hạn một Ticket đang quá hạn tới ngày sau; kiểm nhãn hiện tại và lịch sử cũ.

Expected Result: Gia hạn Ticket đang quá hạn chỉ làm thay nhãn hiện tại, không xóa lịch sử thay hạn.

### FR-SLA-03 Xác định và hiển thị Ticket quá hạn

**TS-SLA-03-01 — AC-SLA-03-01**

Thao tác: Đọc cùng Ticket mở tại clock đúng due_at và due_at cộng 1 giây.

Expected Result: Tại as_of_at bằng due_at, is_overdue false; vượt một giây thì true với Ticket mở.

**TS-SLA-03-02 — AC-SLA-03-02**

Thao tác: Đặt WAITING_INFO với due_at quá khứ; đọc chi tiết và báo cáo.

Expected Result: WAITING_INFO vẫn được tính quá hạn nếu vượt hạn.

**TS-SLA-03-03 — AC-SLA-03-03**

Thao tác: Đọc RESOLVED, REJECTED, CLOSED có due_at quá khứ.

Expected Result: RESOLVED, REJECTED, CLOSED không bị đếm quá hạn đang mở.

**TS-SLA-03-04 — AC-SLA-03-04**

Thao tác: Quét cùng Ticket mở vượt hạn mười lần; đếm OVERDUE_DETECTED và notification.

Expected Result: Một ticket_id và due_at chỉ có một sự kiện cảnh báo, kể cả quét nhiều lần.

**TS-SLA-03-05 — AC-SLA-03-05**

Thao tác: Sau một cảnh báo, QL đổi sang hạn tương lai; vượt hạn mới và quét; kiểm đúng một event cho mỗi hạn.

Expected Result: Gia hạn tạo khóa hạn mới; nếu hạn mới bị vượt có thể có một cảnh báo mới.

**TS-SLA-03-06 — AC-SLA-03-06**

Thao tác: So status trước/sau quét và so nhãn với danh sách RPT-02 tại cùng as_of_at.

Expected Result: Nhãn không đổi status và khớp danh sách báo cáo quá hạn cùng as_of_at.

### FR-FDB-01 Sinh viên xác nhận đóng Ticket

**TS-FDB-01-01 — AC-FDB-01-01**

Thao tác: SV-A xác nhận đóng lần lượt Ticket RESOLVED và REJECTED của mình.

Expected Result: RESOLVED hoặc REJECTED của mình đóng được thành CLOSED với closed_at từ máy chủ.

**TS-FDB-01-02 — AC-FDB-01-02**

Thao tác: So outcome, ended_at và nội dung kết quả trước/sau CLOSED.

Expected Result: outcome, ended_at, resolution hoặc rejection_reason được giữ nguyên.

**TS-FDB-01-03 — AC-FDB-01-03**

Thao tác: SV-B đóng Ticket SV-A; SV-A đóng RECEIVED, PROCESSING, WAITING_INFO.

Expected Result: Không đóng Ticket của SV khác hoặc còn RECEIVED, PROCESSING, WAITING_INFO.

**TS-FDB-01-04 — AC-FDB-01-04**

Thao tác: Đóng Ticket mà không điền điểm; kiểm feedback rỗng.

Expected Result: Đóng không tự tạo đánh giá và không yêu cầu chọn điểm.

**TS-FDB-01-05 — AC-FDB-01-05**

Thao tác: Retry đóng cùng operation_id; đếm TICKET_CLOSED và so closed_at.

Expected Result: Retry chỉ có một TICKET_CLOSED và một mốc closed_at.

**TS-FDB-01-06 — AC-FDB-01-06**

Thao tác: Sau CLOSED, thử mở lại, progress, trả lời bổ sung, điều phối và đổi hạn.

Expected Result: CLOSED không mở lại hoặc tiếp tục xử lý trong phiên bản này.

### FR-FDB-02 Sinh viên gửi đánh giá mức hài lòng

**TS-FDB-02-01 — AC-FDB-02-01**

Thao tác: SV-A đánh giá CLOSED bằng 1 và 5 ở hai Ticket khác nhau; kiểm status và chủ.

Expected Result: Điểm 1 hoặc 5 được lưu đúng Ticket, SV và thời điểm; trạng thái giữ CLOSED.

**TS-FDB-02-02 — AC-FDB-02-02**

Thao tác: Thử điểm thiếu, 0, 6, 2.5 và comment 1001 ký tự; kiểm không có feedback.

Expected Result: 0, 6, 2.5, điểm thiếu và nhận xét 1001 ký tự bị từ chối.

**TS-FDB-02-03 — AC-FDB-02-03**

Thao tác: Đánh giá RESOLVED chưa đóng và CLOSED của SV khác.

Expected Result: Không đánh giá trước CLOSED hoặc Ticket của SV khác.

**TS-FDB-02-04 — AC-FDB-02-04**

Thao tác: Retry cùng mã, rồi gửi mã khác cho cùng Ticket; kiểm một feedback và giá trị đầu.

Expected Result: Retry cùng thao tác trả đánh giá cũ; hai thao tác khác chỉ lưu một đánh giá.

**TS-FDB-02-05 — AC-FDB-02-05**

Thao tác: Sau đánh giá thử sửa hoặc xóa feedback bằng UI và endpoint nghiệp vụ.

Expected Result: Sau gửi không sửa hoặc xóa feedback trong giao diện nghiệp vụ.

**TS-FDB-02-06 — AC-FDB-02-06**

Thao tác: Đóng Ticket REJECTED rồi gửi điểm 2; chạy RPT-06 để xác nhận được tính.

Expected Result: Ticket có outcome REJECTED vẫn được đánh giá.

### FR-RPT-01 Thống kê Ticket theo trạng thái

**TS-RPT-01-01 — AC-RPT-01-01**

Thao tác: Chạy bộ dữ liệu QA-RPT ở QL-A, khoảng ngày 2026-10-12; so sáu nhóm với tổng 9.

Expected Result: Tổng sáu nhóm bằng tổng Ticket cùng bộ lọc.

**TS-RPT-01-02 — AC-RPT-01-02**

Thao tác: Đóng một Ticket RESOLVED; chạy lại và kiểm chỉ chuyển từ nhóm RESOLVED sang CLOSED.

Expected Result: CLOSED được đếm riêng, không đếm thêm vào RESOLVED hay REJECTED.

**TS-RPT-01-03 — AC-RPT-01-03**

Thao tác: Chạy khoảng ngày không có Ticket; kiểm đủ sáu nhóm bằng 0.

Expected Result: Nhóm không có Ticket trả 0.

**TS-RPT-01-04 — AC-RPT-01-04**

Thao tác: Thêm nhiều audit event và câu hỏi vào một Ticket; chạy lại báo cáo.

Expected Result: Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.

**TS-RPT-01-05 — AC-RPT-01-05**

Thao tác: QL-A sửa department_id B; gửi ngày đảo ngược và ngày thiếu ở lượt riêng.

Expected Result: Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

### FR-RPT-02 Xem danh sách Ticket quá hạn đang mở

**TS-RPT-02-01 — AC-RPT-02-01**

Thao tác: Đọc bộ QA-RPT tại as_of_at 2026-10-14 09:00; Q01 đúng hạn không vào danh sách.

Expected Result: Ticket đúng hạn tại as_of_at không vào danh sách.

**TS-RPT-02-02 — AC-RPT-02-02**

Thao tác: Kiểm Q04 WAITING_INFO quá hạn có trong danh sách; Q05 đến Q09 kết thúc không có.

Expected Result: WAITING_INFO vượt hạn được tính; Ticket kết thúc không được tính.

**TS-RPT-02-03 — AC-RPT-02-03**

Thao tác: QL-A chạy bộ QA-RPT: danh sách Q02, Q04, tổng 2; kiểm thứ tự due_at rồi ticket_id.

Expected Result: Danh sách và số quá hạn dùng cùng as_of_at và tổng bằng số dòng đủ điều kiện.

**TS-RPT-02-04 — AC-RPT-02-04**

Thao tác: Thêm nhiều event cho Q02; chạy lại và kiểm chỉ một dòng.

Expected Result: Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.

**TS-RPT-02-05 — AC-RPT-02-05**

Thao tác: QL-A yêu cầu B hoặc ngày đảo ngược; kiểm chặn.

Expected Result: Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

### FR-RPT-03 Tính thời gian giải quyết thành công trung bình

**TS-RPT-03-01 — AC-RPT-03-01**

Thao tác: Lấy hai Ticket outcome RESOLVED kết thúc sau 2 và 4 giờ; kiểm 3.0 giờ, mẫu 2.

Expected Result: Ticket thành công sau 2 giờ và 4 giờ cho trung bình 3.0 giờ, mẫu 2.

**TS-RPT-03-02 — AC-RPT-03-02**

Thao tác: Trong QA-RPT A, kiểm Q05, Q07, Q09 vào mẫu; Q06, Q08 không vào.

Expected Result: CLOSED với outcome RESOLVED vẫn được tính; outcome REJECTED không được tính.

**TS-RPT-03-03 — AC-RPT-03-03**

Thao tác: Chạy khoảng chỉ có outcome REJECTED hoặc Ticket còn mở; kiểm NULL và số mẫu 0.

Expected Result: Không có mẫu hợp lệ trả NULL, không trả 0 giờ.

**TS-RPT-03-04 — AC-RPT-03-04**

Thao tác: Dịch closed_at sau ended_at 2 ngày và tạo vòng WAITING_INFO trước kết quả; kiểm thời gian không dùng closed_at hoặc trừ thời gian chờ.

Expected Result: Không dùng closed_at thay ended_at và không trừ thời gian WAITING_INFO.

**TS-RPT-03-05 — AC-RPT-03-05**

Thao tác: Thêm history nhiều dòng cho Q07; chạy lại không tăng số mẫu.

Expected Result: Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.

**TS-RPT-03-06 — AC-RPT-03-06**

Thao tác: QL-A yêu cầu B trực tiếp hoặc bộ lọc ngày không hợp lệ.

Expected Result: Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

### FR-RPT-04 Thống kê Ticket theo loại vấn đề

**TS-RPT-04-01 — AC-RPT-04-01**

Thao tác: Chạy QA-RPT A: A1 bằng 4, A2 bằng 5, tổng 9; so thứ tự.

Expected Result: Tổng các loại bằng tổng Ticket trong bộ lọc.

**TS-RPT-04-02 — AC-RPT-04-02**

Thao tác: Đổi loại một Ticket mở, rồi chuyển một Ticket sang B; chạy lại dùng loại và quyền hiện tại.

Expected Result: Phân loại lại hoặc chuyển phòng ban dùng loại hiện tại và phạm vi hiện tại.

**TS-RPT-04-03 — AC-RPT-04-03**

Thao tác: Chạy ngày rỗng; kiểm trạng thái Không có dữ liệu.

Expected Result: Loại hiện không có Ticket có thể ẩn; tập rỗng hiển thị Không có dữ liệu.

**TS-RPT-04-04 — AC-RPT-04-04**

Thao tác: Thêm progress và information request vào một Ticket; chạy lại tổng không tăng.

Expected Result: Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.

**TS-RPT-04-05 — AC-RPT-04-05**

Thao tác: QL-A gửi category/department ngoài quyền hoặc ngày sai.

Expected Result: Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

### FR-RPT-05 Thống kê tải công việc theo nhân viên

**TS-RPT-05-01 — AC-RPT-05-01**

Thao tác: Chạy QA-RPT A: tổng tải 4; so tất cả nhóm.

Expected Result: Tổng nhóm bằng số Ticket mở cùng bộ lọc.

**TS-RPT-05-02 — AC-RPT-05-02**

Thao tác: Kiểm Q02 RECEIVED đã giao, Q03 PROCESSING vào NV-A1; Q04 WAITING_INFO vào NV-A2.

Expected Result: RECEIVED đã phân công, PROCESSING và WAITING_INFO tính vào người hiện tại.

**TS-RPT-05-03 — AC-RPT-05-03**

Thao tác: Kiểm Q05 đến Q09 không tính tải mở dù còn assignee.

Expected Result: RESOLVED, REJECTED, CLOSED không tính tải mở.

**TS-RPT-05-04 — AC-RPT-05-04**

Thao tác: Đổi Q03 từ NV-A1 sang NV-A2 rồi chuyển phòng ban; kiểm từng nhóm thay đúng một.

Expected Result: Đổi người chuyển đúng một Ticket từ nhóm cũ sang nhóm mới; chuyển phòng ban đưa về Chưa phân công ở đích.

**TS-RPT-05-05 — AC-RPT-05-05**

Thao tác: Thêm nhiều lịch sử cho Q02; chạy lại tải không tăng.

Expected Result: Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.

**TS-RPT-05-06 — AC-RPT-05-06**

Thao tác: QL-A sửa phòng ban B; gửi ngày đảo ngược hoặc thiếu.

Expected Result: Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

### FR-RPT-06 Thống kê mức hài lòng của sinh viên

**TS-RPT-06-01 — AC-RPT-06-01**

Thao tác: Lấy hai feedback 3 và 5; kiểm mean 4.0 và mẫu 2.

Expected Result: Điểm 3 và 5 cho trung bình 4.0, số mẫu 2.

**TS-RPT-06-02 — AC-RPT-06-02**

Thao tác: Chạy QA-RPT A gồm 3 CLOSED nhưng Q09 không feedback; kiểm không tính 0 vào mean.

Expected Result: Ticket CLOSED chưa đánh giá không được coi là điểm 0.

**TS-RPT-06-03 — AC-RPT-06-03**

Thao tác: Kiểm phân phối điểm 1 đến 5: điểm 3 và 5 mỗi điểm 1, còn lại 0.

Expected Result: Phân phối đủ các điểm 1 đến 5; tổng bằng số đánh giá.

**TS-RPT-06-04 — AC-RPT-06-04**

Thao tác: Chạy tập không có CLOSED rồi tập có CLOSED chưa feedback; kiểm NULL và 0.0% đúng từng mẫu.

Expected Result: Không có CLOSED: tỷ lệ NULL; có CLOSED nhưng chưa feedback: tỷ lệ 0.0%.

**TS-RPT-06-05 — AC-RPT-06-05**

Thao tác: Kiểm feedback điểm 5 của Q08 outcome REJECTED vẫn được tính.

Expected Result: Đánh giá Ticket outcome REJECTED được tính, không lọc bỏ.

**TS-RPT-06-06 — AC-RPT-06-06**

Thao tác: Thêm nhiều event cho Q08; chạy lại số feedback và mean không thay.

Expected Result: Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.

**TS-RPT-06-07 — AC-RPT-06-07**

Thao tác: QL-A yêu cầu B hoặc from_date lớn hơn to_date; kiểm chặn.

Expected Result: Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

[Về danh mục PRD](../README.md)
