# M07 Đặc tả Thông báo

## [FR-NOT-01] Tạo thông báo trong ứng dụng theo sự kiện

**Mô tả**

Tạo thông báo cho người liên quan sau thay đổi nghiệp vụ đã commit theo bảng sự kiện và người nhận.

**Actor**

Hệ thống

**Preconditions**

- Sự kiện nghiệp vụ thành công đã được lưu cùng giao dịch; người nhận hoạt động theo bảng định tuyến.

**Luồng chính**

1. Thao tác nghiệp vụ lưu một domain event có event_id và danh sách người nhận được chụp tại commit.
2. Bộ xử lý thông báo tạo từng notification theo khóa duy nhất event_id và recipient_id.
3. Lưu unread, thông điệp tối thiểu và ticket_id; đánh dấu phần xử lý thành công để có thể retry nếu lỗi.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| event_id, recipient_id | Khóa duy nhất | Một thông báo cho mỗi người của một sự kiện. |
| payload | Hệ thống tạo | Mã Ticket và hành động; không đưa mô tả riêng tư vào thông báo. |
| created_at, read_at | Thời điểm | read_at rỗng khi chưa đọc. |

**Business Rules**

- Điều kiện nghiệp vụ: Sự kiện nghiệp vụ thành công đã được lưu cùng giao dịch; người nhận hoạt động theo bảng định tuyến.
- Kết quả bắt buộc: Thêm notification; không đổi trạng thái Ticket.
- Quy tắc dùng chung: BR-08, BR-13. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Bộ xử lý tạm lỗi: Ticket vẫn thành công; event chờ retry, lỗi được ghi log.
- Một người nhận xuất hiện qua nhiều vai trò: hợp nhất thành một thông báo.
- Không còn quyền Ticket: vẫn có thông báo tối thiểu, mở nội dung kiểm quyền hiện tại.

**Acceptance Criteria**

- **AC-NOT-01-01** Mỗi sự kiện thuộc bảng định tuyến tạo đúng tập người nhận được định nghĩa.
- **AC-NOT-01-02** Retry cùng event_id và recipient_id không tạo thông báo thứ hai.
- **AC-NOT-01-03** Giao dịch Ticket rollback không tạo thông báo.
- **AC-NOT-01-04** Một recipient thất bại không làm mất các recipient còn lại; lần retry chỉ bù phần thiếu.
- **AC-NOT-01-05** Thông báo hiển thị trong 60 giây khi bộ xử lý hoạt động.
- **AC-NOT-01-06** Nội dung chỉ có mã Ticket, tên hành động và thời điểm; không có mô tả nhạy cảm.

**Ví dụ Edge Case**

Một QL quản lý cả phòng ban nguồn và đích khi chuyển Ticket.

**Expected Result**

QL đó chỉ nhận một thông báo cho sự kiện chuyển.

**Hiệu ứng dữ liệu**

Thêm notification; không đổi trạng thái Ticket.

**Yêu cầu giao diện**

Thông báo nội ứng dụng; xử lý theo polling, không yêu cầu realtime push.

**Liên kết thực hiện và kiểm thử**

Module M07; ưu tiên Must. Tiền đề triển khai: FR-STU-02. Workflow: WF-01, WF-02, WF-03, WF-04, WF-05, WF-06. Kịch bản: TS-NOT-01-01 đến TS-NOT-01-06.



## [FR-NOT-02] Xem danh sách thông báo của tôi

**Mô tả**

Đọc thông báo của người dùng hiện tại và mở Ticket khi vẫn còn quyền.

**Actor**

SV, NV, QL

**Preconditions**

- Người dùng có phiên hợp lệ.

**Luồng chính**

1. Người dùng mở Thông báo và chọn Tất cả hoặc Chưa đọc.
2. Máy chủ lấy notification theo recipient_id từ phiên, created_at giảm dần rồi notification_id giảm dần.
3. Hiển thị danh sách, số chưa đọc; mở Ticket qua kiểm quyền hiện tại.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| filter | Enum tùy chọn | ALL hoặc UNREAD; mặc định ALL. |
| page | Số nguyên | Từ 1; 20 thông báo mỗi trang. |

**Business Rules**

- Điều kiện nghiệp vụ: Người dùng có phiên hợp lệ.
- Kết quả bắt buộc: Chỉ đọc; đánh dấu đọc thuộc FR-NOT-03.
- Quy tắc dùng chung: BR-01, BR-13. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Chưa có thông báo: trạng thái rỗng.
- Không còn quyền Ticket: Không còn quyền truy cập Ticket; vẫn thấy thông báo tối thiểu của mình.

**Acceptance Criteria**

- **AC-NOT-02-01** Chỉ nhận được notification của tài khoản trong phiên.
- **AC-NOT-02-02** UNREAD lọc read_at rỗng; số chưa đọc khớp cùng tài khoản.
- **AC-NOT-02-03** Mở Ticket kiểm quyền hiện tại, không dùng quyền lúc gửi thông báo.
- **AC-NOT-02-04** Xem danh sách chưa tự đánh dấu tất cả đã đọc.
- **AC-NOT-02-05** Không có thông báo hiển thị Bạn chưa có thông báo.

**Ví dụ Edge Case**

NV cũ mở thông báo phân công sau khi Ticket được chuyển.

**Expected Result**

Không đọc được Ticket; thông báo tối thiểu vẫn thuộc NV cũ.

**Hiệu ứng dữ liệu**

Chỉ đọc; đánh dấu đọc thuộc FR-NOT-03.

**Yêu cầu giao diện**

Bộ lọc, danh sách thông báo, nhãn chưa đọc, số chưa đọc.

**Liên kết thực hiện và kiểm thử**

Module M07; ưu tiên Must. Tiền đề triển khai: FR-NOT-01, FR-IAM-03. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-NOT-02-01 đến TS-NOT-02-05.



## [FR-NOT-03] Đánh dấu một thông báo đã đọc

**Mô tả**

Ghi nhận người nhận đã đọc một thông báo riêng lẻ.

**Actor**

SV, NV, QL

**Preconditions**

- Thông báo thuộc người dùng trong phiên.

**Luồng chính**

1. Người dùng chọn Đánh dấu đã đọc hoặc mở thông báo.
2. Máy chủ kiểm recipient_id và cập nhật read_at nếu đang rỗng.
3. Giảm số chưa đọc, trả trạng thái đã đọc.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| notification_id | Khóa bắt buộc | Thông báo của tài khoản hiện tại. |

**Business Rules**

- Điều kiện nghiệp vụ: Thông báo thuộc người dùng trong phiên.
- Kết quả bắt buộc: Chỉ đổi read_at của notification.
- Quy tắc dùng chung: BR-01, BR-13. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngoài chủ thông báo hoặc không tồn tại: từ chối.
- Đã đọc: trả thành công, giữ read_at cũ.

**Acceptance Criteria**

- **AC-NOT-03-01** Thông báo chưa đọc của mình được ghi read_at bằng thời điểm máy chủ.
- **AC-NOT-03-02** Đánh dấu lần hai không thay read_at đầu tiên.
- **AC-NOT-03-03** Không đánh dấu được thông báo của tài khoản khác.
- **AC-NOT-03-04** Số chưa đọc giảm đúng một và không âm.
- **AC-NOT-03-05** Thông báo được đánh dấu đã đọc dù liên kết Ticket hiện không còn quyền; việc đọc Ticket vẫn bị chặn.

**Ví dụ Edge Case**

Người dùng bấm đánh dấu ở hai tab.

**Expected Result**

read_at chỉ có một mốc đầu tiên; số chưa đọc không bị trừ hai lần.

**Hiệu ứng dữ liệu**

Chỉ đổi read_at của notification.

**Yêu cầu giao diện**

Nút đánh dấu từng thông báo; không có đánh dấu hàng loạt trong phạm vi này.

**Liên kết thực hiện và kiểm thử**

Module M07; ưu tiên Must. Tiền đề triển khai: FR-NOT-02. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-NOT-03-01 đến TS-NOT-03-05.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
