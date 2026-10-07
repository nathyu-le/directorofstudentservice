# M02 Đặc tả Yêu cầu của sinh viên

## [FR-STU-01] Xem danh sách yêu cầu của tôi

**Mô tả**

Tra cứu danh sách Ticket do sinh viên hiện tại gửi.

**Actor**

SV

**Preconditions**

- SV có phiên hợp lệ.

**Luồng chính**

1. SV mở Yêu cầu của tôi, nhập từ khóa hoặc chọn trạng thái.
2. Máy chủ giới hạn student_id theo phiên rồi áp dụng đồng thời bộ lọc.
3. Trả danh sách theo created_at giảm dần, cùng thời điểm theo ticket_id giảm dần.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| keyword | Tùy chọn | Tối đa 150 ký tự; tìm mã hoặc tiêu đề không phân biệt hoa thường. |
| status | Tùy chọn | Một trong sáu trạng thái hoặc Tất cả. |
| page | Số nguyên | Từ 1; mặc định 1; 20 Ticket mỗi trang. |

**Business Rules**

- Điều kiện nghiệp vụ: SV có phiên hợp lệ.
- Kết quả bắt buộc: Chỉ đọc.
- Quy tắc dùng chung: BR-01, BR-12. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Bộ lọc trạng thái sai: báo bộ lọc không hợp lệ.
- Không có kết quả: hiển thị danh sách rỗng, không báo lỗi.

**Acceptance Criteria**

- **AC-STU-01-01** Chỉ hiển thị Ticket của SV trong phiên, kể cả khi sửa tham số student_id.
- **AC-STU-01-02** Kết hợp từ khóa và trạng thái trả đúng tập kết quả.
- **AC-STU-01-03** Tổng trang và danh sách dùng cùng bộ lọc và quyền.
- **AC-STU-01-04** Đổi trang giữ từ khóa và trạng thái; thứ tự ổn định.
- **AC-STU-01-05** Không có Ticket hiển thị Chưa có yêu cầu và liên kết Tạo Ticket.

**Ví dụ Edge Case**

Hai Ticket có cùng created_at nằm ở ranh giới hai trang.

**Expected Result**

Sắp bổ sung theo ticket_id; cùng một tập dữ liệu không bị lặp hoặc mất dòng.

**Hiệu ứng dữ liệu**

Chỉ đọc.

**Yêu cầu giao diện**

Bảng mã, tiêu đề, trạng thái, phòng ban, người phụ trách, hạn; liên kết chi tiết.

**Liên kết thực hiện và kiểm thử**

Module M02; ưu tiên Must. Tiền đề triển khai: FR-IAM-03, FR-STU-02. Workflow: WF-01. Kịch bản: TS-STU-01-01 đến TS-STU-01-05.



## [FR-STU-02] Sinh viên gửi yêu cầu hỗ trợ

**Mô tả**

Sinh viên tạo một Ticket bằng loại vấn đề, tiêu đề và mô tả; hệ thống cấp mã xác nhận duy nhất.

**Actor**

SV

**Preconditions**

- SV đã đăng nhập và có quyền tạo Ticket.
- Loại vấn đề và phòng ban tiếp nhận đang hoạt động.

**Luồng chính**

1. SV mở Tạo Ticket; hệ thống hiển thị form và cấp operation_id cho lần gửi.
2. SV chọn loại, nhập tiêu đề, mô tả và chọn Gửi yêu cầu.
3. Máy chủ kiểm dữ liệu, suy ra phòng ban từ loại vấn đề và kiểm mã chống trùng.
4. Trong một giao dịch, tạo Ticket RECEIVED chưa phân công, tính hạn, ghi sự kiện và lưu sự kiện thông báo.
5. Trả mã Ticket và liên kết chi tiết; chỉ báo thành công sau khi lưu đủ.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| category_id | Khóa bắt buộc | Loại đang hoạt động; xác định department_id tại máy chủ. |
| title | Chuỗi bắt buộc | 5 đến 150 ký tự sau trim. |
| description | Văn bản bắt buộc | 10 đến 3000 ký tự sau trim; không chỉ có khoảng trắng. |
| operation_id | UUID bắt buộc | Cùng một lần gửi giữ cùng mã khi double click hoặc retry. |

**Business Rules**

- Điều kiện nghiệp vụ: SV đã đăng nhập và có quyền tạo Ticket. Loại vấn đề và phòng ban tiếp nhận đang hoạt động.
- Kết quả bắt buộc: Tạo Ticket RECEIVED và sự kiện TICKET_CREATED; tích hợp FR-SLA-01 và FR-NOT-01.
- Quy tắc dùng chung: BR-03, BR-08, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Thiếu, dưới hoặc trên giới hạn: không tạo Ticket, lỗi tại trường.
- Danh mục không còn hoạt động: yêu cầu chọn lại.
- Lưu thất bại: rollback toàn bộ, giữ dữ liệu form để thử lại.
- Mã thao tác đã dùng với payload khác: báo xung đột.

**Acceptance Criteria**

- **AC-STU-02-01** Dữ liệu hợp lệ tạo đúng một Ticket, RECEIVED, assignee_id rỗng, mã duy nhất.
- **AC-STU-02-02** student_id lấy từ phiên, department_id lấy từ danh mục, created_at và due_at lấy từ máy chủ.
- **AC-STU-02-03** Mô tả trống hoặc chỉ khoảng trắng không tạo Ticket.
- **AC-STU-02-04** Tiêu đề 4 hoặc 151 ký tự, mô tả 9 hoặc 3001 ký tự bị từ chối; giá trị biên hợp lệ được nhận.
- **AC-STU-02-05** Bấm Gửi nhiều lần cùng operation_id chỉ có một Ticket, một sự kiện tạo và cùng mã xác nhận.
- **AC-STU-02-06** Retry sau mất kết nối trả Ticket đã tạo, không tạo thêm.
- **AC-STU-02-07** Một phần ghi thất bại không để Ticket, lịch sử hoặc thông báo mồ côi.

**Ví dụ Edge Case**

SV bấm Gửi năm lần liên tiếp, response đầu tiên bị mất.

**Expected Result**

Một Ticket duy nhất; tất cả lần retry cùng thao tác nhận lại cùng mã Ticket.

**Hiệu ứng dữ liệu**

Tạo Ticket RECEIVED và sự kiện TICKET_CREATED; tích hợp FR-SLA-01 và FR-NOT-01.

**Yêu cầu giao diện**

Loại vấn đề, tiêu đề, mô tả; nút Gửi; xác nhận có mã Ticket.

**Liên kết thực hiện và kiểm thử**

Module M02; ưu tiên Must. Tiền đề triển khai: FR-IAM-03. Workflow: WF-01. Kịch bản: TS-STU-02-01 đến TS-STU-02-07.



## [FR-STU-03] Xem chi tiết và tiến độ yêu cầu của tôi

**Mô tả**

Đọc thông tin hiện hành và kết quả của Ticket thuộc sinh viên; liên kết đến hội thoại, lịch sử và thao tác được phép.

**Actor**

SV

**Preconditions**

- SV đăng nhập; Ticket thuộc SV hiện tại.

**Luồng chính**

1. SV mở Ticket từ danh sách hoặc liên kết.
2. Máy chủ kiểm chủ Ticket và tải phiên bản hiện hành.
3. Hiển thị nội dung, trạng thái, trách nhiệm, hạn, yêu cầu bổ sung đang mở hoặc kết quả; cung cấp liên kết theo quyền.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | Ticket của chính SV. |
| version | Chỉ đọc | Giúp form con gửi đúng phiên bản. |
| outcome | Chỉ đọc, có thể rỗng | RESOLVED hoặc REJECTED được giữ khi CLOSED. |

**Business Rules**

- Điều kiện nghiệp vụ: SV đăng nhập; Ticket thuộc SV hiện tại.
- Kết quả bắt buộc: Chỉ đọc; hội thoại chi tiết do FR-COM-03, lịch sử do FR-TKT-04.
- Quy tắc dùng chung: BR-01, BR-04, BR-07. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Không tồn tại hoặc sai chủ: Không tìm thấy Ticket.
- Lỗi tải: cho thử lại; không hiển thị trạng thái tự suy đoán.

**Acceptance Criteria**

- **AC-STU-03-01** Tải lại hiển thị cùng trạng thái, phòng ban, người phụ trách và hạn với dữ liệu máy chủ.
- **AC-STU-03-02** Chưa phân công hiển thị Chưa phân công; quá hạn là nhãn riêng, không thay trạng thái.
- **AC-STU-03-03** WAITING_INFO hiển thị yêu cầu bổ sung đang mở; chỉ cho liên kết trả lời tương ứng.
- **AC-STU-03-04** RESOLVED hoặc REJECTED hiển thị nội dung kết quả và nút xác nhận đóng.
- **AC-STU-03-05** CLOSED giữ nội dung kết quả và cho đánh giá nếu chưa có đánh giá.
- **AC-STU-03-06** Đường dẫn của SV khác không lộ tiêu đề, nội dung, hội thoại hoặc lịch sử.

**Ví dụ Edge Case**

SV đang mở chi tiết khi QL chuyển Ticket sang phòng ban khác.

**Expected Result**

Sau tải lại, hiển thị phòng ban mới và Chưa phân công; giữ nguyên mã và nội dung gốc.

**Hiệu ứng dữ liệu**

Chỉ đọc; hội thoại chi tiết do FR-COM-03, lịch sử do FR-TKT-04.

**Yêu cầu giao diện**

Mã và trạng thái ở đầu; thông tin gốc, trách nhiệm, hạn, kết quả bên dưới.

**Liên kết thực hiện và kiểm thử**

Module M02; ưu tiên Must. Tiền đề triển khai: FR-STU-02, FR-IAM-03. Workflow: WF-01, WF-03, WF-06. Kịch bản: TS-STU-03-01 đến TS-STU-03-06.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
