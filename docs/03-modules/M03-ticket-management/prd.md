# M03 Đặc tả Quản lý Ticket

## [FR-TKT-01] Xem hàng chờ và tìm kiếm Ticket

**Mô tả**

Tra cứu Ticket trong phạm vi nội bộ, phân biệt hàng chờ tiếp nhận và công việc đang được giao.

**Actor**

NV, QL

**Preconditions**

- NV hoặc QL có phiên hợp lệ và phạm vi phòng ban.

**Luồng chính**

1. Người dùng mở Hàng chờ hoặc Công việc của tôi và chọn bộ lọc.
2. Máy chủ giới hạn quyền trước khi tìm theo mã, tiêu đề, trạng thái, loại, phòng ban, người phụ trách.
3. Hiển thị 20 dòng mỗi trang, created_at tăng dần rồi ticket_id tăng dần; chọn dòng mở chi tiết nội bộ.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| view | Bắt buộc | NV: Hàng chờ hoặc Công việc của tôi; QL: phạm vi quản lý. |
| filters | Tùy chọn | keyword tối đa 150 ký tự; status, category_id, department_id, assignment. |
| page | Số nguyên | Từ 1; 20 dòng mỗi trang. |

**Business Rules**

- Điều kiện nghiệp vụ: NV hoặc QL có phiên hợp lệ và phạm vi phòng ban.
- Kết quả bắt buộc: Chỉ đọc.
- Quy tắc dùng chung: BR-01, BR-02, BR-12. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Bộ lọc ngoài phạm vi: từ chối, không bỏ qua âm thầm.
- Không có kết quả: Không có Ticket phù hợp.

**Acceptance Criteria**

- **AC-TKT-01-01** NV ở Hàng chờ chỉ thấy RECEIVED, assignee_id rỗng, đúng phòng ban mình.
- **AC-TKT-01-02** NV ở Công việc của tôi thấy Ticket đang được giao cho mình, kể cả đã có kết quả.
- **AC-TKT-01-03** QL thấy Ticket thuộc tập phòng ban được quản lý và đúng bộ lọc.
- **AC-TKT-01-04** Nhận hoặc phân công thành công làm Ticket rời tập chưa phân công khi tải lại.
- **AC-TKT-01-05** Tổng số dòng, trang và kết quả cùng tuân thủ quyền và bộ lọc.

**Ví dụ Edge Case**

NV đổi department_id trong bộ lọc sang phòng ban khác.

**Expected Result**

Không nhận được Ticket của phòng ban khác.

**Hiệu ứng dữ liệu**

Chỉ đọc.

**Yêu cầu giao diện**

Bộ lọc và bảng mã, SV, loại, trạng thái, phòng ban, người phụ trách, hạn; nút Nhận theo quyền.

**Liên kết thực hiện và kiểm thử**

Module M03; ưu tiên Must. Tiền đề triển khai: FR-IAM-03, FR-STU-02. Workflow: WF-02, WF-04. Kịch bản: TS-TKT-01-01 đến TS-TKT-01-05.



## [FR-TKT-02] Điều chỉnh loại vấn đề của Ticket

**Mô tả**

Đổi loại vấn đề trong cùng phòng ban để bảo đảm phân loại đúng, không thay đổi trách nhiệm hoặc hạn.

**Actor**

QL

**Preconditions**

- Ticket thuộc phạm vi QL, ở RECEIVED hoặc PROCESSING.
- Loại mới hoạt động và thuộc phòng ban hiện tại.

**Luồng chính**

1. QL mở phần Phân loại, chọn loại mới và nhập lý do.
2. Máy chủ kiểm quyền, trạng thái, loại cùng phòng ban và version.
3. Lưu loại mới, tăng version và ghi lịch sử loại trước và sau.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| category_id | Khóa bắt buộc | Cùng department_id hiện tại; đang hoạt động. |
| reason | Văn bản bắt buộc khi đổi | 10 đến 500 ký tự. |
| version, operation_id | Bắt buộc | Theo BR-09 và BR-10. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket thuộc phạm vi QL, ở RECEIVED hoặc PROCESSING. Loại mới hoạt động và thuộc phòng ban hiện tại.
- Kết quả bắt buộc: Đổi category_id; giữ hạn đã chụp khi tạo Ticket.
- Quy tắc dùng chung: BR-05, BR-08, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Loại thuộc phòng ban khác: hướng dẫn dùng chuyển phòng ban.
- Loại giữ nguyên: trả Không có thay đổi; không thêm sự kiện.
- WAITING_INFO hoặc Ticket kết thúc: từ chối đổi loại.

**Acceptance Criteria**

- **AC-TKT-02-01** Loại hợp lệ được đổi, giữ status, department_id, assignee_id và due_at.
- **AC-TKT-02-02** Ghi một sự kiện CATEGORY_CHANGED có loại cũ, mới, lý do và actor.
- **AC-TKT-02-03** Loại khác phòng ban hoặc ngừng hoạt động bị từ chối.
- **AC-TKT-02-04** Lý do trống, dưới 10 hoặc trên 500 ký tự không lưu.
- **AC-TKT-02-05** Version cũ không ghi đè phân loại vừa được cập nhật.

**Ví dụ Edge Case**

Danh mục bị vô hiệu hóa sau khi QL mở form.

**Expected Result**

Máy chủ kiểm lại danh mục và từ chối lưu.

**Hiệu ứng dữ liệu**

Đổi category_id; giữ hạn đã chụp khi tạo Ticket.

**Yêu cầu giao diện**

Loại hiện tại, loại mới, lý do; nhắc hạn hiện tại được giữ.

**Liên kết thực hiện và kiểm thử**

Module M03; ưu tiên Must. Tiền đề triển khai: FR-TKT-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-TKT-02-01 đến TS-TKT-02-05.



## [FR-TKT-03] Xem chi tiết Ticket nội bộ

**Mô tả**

Cung cấp màn hình Ticket cho tra cứu nội bộ và các thao tác xử lý hoặc điều phối theo quyền.

**Actor**

NV, QL

**Preconditions**

- Ticket nằm trong phạm vi đọc của NV hoặc QL theo FR-IAM-03.

**Luồng chính**

1. Người dùng chọn Ticket.
2. Máy chủ kiểm quyền hiện hành và tải dữ liệu hiện tại.
3. Hiển thị dữ liệu Ticket và chỉ cung cấp các nút hợp lệ cho vai trò, trạng thái và phân công.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | Trong phạm vi đọc. |
| version | Chỉ đọc | Gửi kèm các thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket nằm trong phạm vi đọc của NV hoặc QL theo FR-IAM-03.
- Kết quả bắt buộc: Chỉ đọc.
- Quy tắc dùng chung: BR-01, BR-02, BR-04. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ticket ngoài quyền hoặc không tồn tại: Không tìm thấy Ticket.
- Phân công thay đổi sau khi tải: thao tác sau phải kiểm quyền lại.

**Acceptance Criteria**

- **AC-TKT-03-01** NV xem được Ticket đang giao cho mình hoặc Ticket RECEIVED chưa phân công của phòng ban mình.
- **AC-TKT-03-02** QL xem được Ticket thuộc phòng ban quản lý.
- **AC-TKT-03-03** Hiển thị đúng version, trạng thái, hạn, người phụ trách và kết quả hiện hành.
- **AC-TKT-03-04** NV chỉ thấy nút xử lý trên Ticket của mình; QL chỉ thấy điều phối theo trạng thái.
- **AC-TKT-03-05** Màn hình đọc không thay trạng thái hoặc tự nhận Ticket.

**Ví dụ Edge Case**

QL xem chi tiết RECEIVED chưa phân công.

**Expected Result**

Chỉ đọc vẫn giữ RECEIVED và assignee_id rỗng.

**Hiệu ứng dữ liệu**

Chỉ đọc.

**Yêu cầu giao diện**

Thông tin Ticket, nội dung xử lý, liên kết Hội thoại và Lịch sử; nhóm nút theo vai trò.

**Liên kết thực hiện và kiểm thử**

Module M03; ưu tiên Must. Tiền đề triển khai: FR-TKT-01, FR-IAM-03. Workflow: WF-02, WF-04, WF-05. Kịch bản: TS-TKT-03-01 đến TS-TKT-03-05.



## [FR-TKT-04] Xem lịch sử nghiệp vụ Ticket

**Mô tả**

Đọc chuỗi sự kiện bất biến để biết ai thực hiện thay đổi nào và lúc nào.

**Actor**

SV, NV, QL

**Preconditions**

- Có quyền đọc Ticket hiện hành.

**Luồng chính**

1. Người dùng mở Lịch sử từ trang chi tiết.
2. Máy chủ kiểm quyền và lấy sự kiện theo occurred_at tăng dần rồi event_id tăng dần.
3. Hiển thị hành động, actor, thời điểm và giá trị trước sau; nội dung trao đổi mở ở Hội thoại.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | Cùng kiểm quyền với chi tiết. |
| events | Chỉ đọc | event_id, event_type, actor, occurred_at, old_value, new_value. |

**Business Rules**

- Điều kiện nghiệp vụ: Có quyền đọc Ticket hiện hành.
- Kết quả bắt buộc: Chỉ đọc.
- Quy tắc dùng chung: BR-01, BR-08. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngoài quyền: không trả lịch sử.
- Chưa có sự kiện: trạng thái rỗng; không dựng lịch sử giả.

**Acceptance Criteria**

- **AC-TKT-04-01** Ticket mới có một TICKET_CREATED; mỗi thay đổi thành công có sự kiện tương ứng.
- **AC-TKT-04-02** Thứ tự ổn định khi hai sự kiện cùng thời điểm.
- **AC-TKT-04-03** Retry cùng thao tác không thêm sự kiện nghiệp vụ.
- **AC-TKT-04-04** Không có thao tác sửa hoặc xóa lịch sử trên giao diện hay endpoint nghiệp vụ.
- **AC-TKT-04-05** NV mất quyền Ticket cũng mất quyền lịch sử; SV không thấy email, session hay dữ liệu bí mật của actor.

**Ví dụ Edge Case**

Retry chuyển phòng ban ba lần cùng operation_id.

**Expected Result**

Lịch sử chỉ có một DEPARTMENT_TRANSFERRED.

**Hiệu ứng dữ liệu**

Chỉ đọc.

**Yêu cầu giao diện**

Dòng thời gian; lý do, trước và sau dưới từng hành động.

**Liên kết thực hiện và kiểm thử**

Module M03; ưu tiên Must. Tiền đề triển khai: FR-STU-02, FR-IAM-03. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-TKT-04-01 đến TS-TKT-04-05.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
