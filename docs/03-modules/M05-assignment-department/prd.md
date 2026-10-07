# M05 Đặc tả Phân công và phòng ban

## [FR-ASG-01] Phân công người phụ trách Ticket

**Mô tả**

Giao lần đầu một Ticket chưa có người phụ trách cho NV hoạt động trong phòng ban hiện tại.

**Actor**

QL

**Preconditions**

- Ticket RECEIVED chưa phân công, thuộc phạm vi QL.
- NV được chọn hoạt động và thuộc phòng ban Ticket.

**Luồng chính**

1. QL chọn Ticket, chọn NV và xác nhận phân công.
2. Máy chủ kiểm trạng thái, NV, version và điều kiện chưa phân công.
3. Lưu assignee_id, ghi ASSIGNEE_ASSIGNED và thông báo cho NV; giữ RECEIVED để NV tự bắt đầu.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| assignee_id | Khóa bắt buộc | NV hoạt động, cùng department_id. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket RECEIVED chưa phân công, thuộc phạm vi QL. NV được chọn hoạt động và thuộc phòng ban Ticket.
- Kết quả bắt buộc: Điền assignee_id; không đổi trạng thái.
- Quy tắc dùng chung: BR-02, BR-04, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ticket đã có người: dùng Đổi người phụ trách.
- NV sai phòng ban hoặc ngừng hoạt động: từ chối.
- Nhận đồng thời đã thắng trước: trả xung đột, không ghi đè.

**Acceptance Criteria**

- **AC-ASG-01-01** Phân công hợp lệ điền đúng assignee_id, giữ RECEIVED và due_at.
- **AC-ASG-01-02** NV được giao thấy Ticket trong Công việc của tôi và nhận thông báo.
- **AC-ASG-01-03** QL không phân công Ticket ngoài phòng ban quản lý.
- **AC-ASG-01-04** Không chọn được NV ngừng hoạt động hoặc khác phòng ban; máy chủ kiểm lại.
- **AC-ASG-01-05** Nhận việc và phân công đồng thời vẫn có tối đa một người phụ trách.
- **AC-ASG-01-06** Retry không thêm lịch sử hoặc thông báo.

**Ví dụ Edge Case**

NV được chọn bị vô hiệu hóa ngay trước lưu.

**Expected Result**

Máy chủ từ chối; Ticket vẫn chưa phân công.

**Hiệu ứng dữ liệu**

Điền assignee_id; không đổi trạng thái.

**Yêu cầu giao diện**

Danh sách NV hoạt động cùng phòng ban; nút Phân công.

**Liên kết thực hiện và kiểm thử**

Module M05; ưu tiên Must. Tiền đề triển khai: FR-TKT-01. Workflow: WF-02, WF-04. Kịch bản: TS-ASG-01-01 đến TS-ASG-01-06.



## [FR-ASG-02] Đổi người phụ trách Ticket

**Mô tả**

Chuyển trách nhiệm từ NV hiện tại sang NV khác trong cùng phòng ban mà giữ diễn biến nghiệp vụ.

**Actor**

QL

**Preconditions**

- Ticket RECEIVED, PROCESSING hoặc WAITING_INFO, đã phân công, thuộc phạm vi QL.

**Luồng chính**

1. QL chọn NV mới và nhập lý do.
2. Máy chủ kiểm NV mới khác NV cũ, cùng phòng ban, hoạt động và version.
3. Đổi assignee_id, ghi giá trị cũ mới, thông báo cho người cũ và mới; quyền xử lý được tính lại ngay.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| assignee_id | Khóa bắt buộc | NV mới hoạt động cùng phòng ban, khác người cũ. |
| reason | Văn bản bắt buộc | 10 đến 500 ký tự. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket RECEIVED, PROCESSING hoặc WAITING_INFO, đã phân công, thuộc phạm vi QL.
- Kết quả bắt buộc: Chỉ đổi assignee_id, không đổi status.
- Quy tắc dùng chung: BR-02, BR-05, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Chưa có người: dùng Phân công.
- Chọn lại người cũ: Không có thay đổi; không thêm sự kiện.
- Ticket đã kết thúc hoặc NV mới không hợp lệ: từ chối.

**Acceptance Criteria**

- **AC-ASG-02-01** Đổi hợp lệ giữ status, department_id, due_at, started_at và câu hỏi đang mở.
- **AC-ASG-02-02** Lịch sử ghi người cũ, mới và lý do.
- **AC-ASG-02-03** Người cũ không còn đọc hay ghi Ticket; người mới được xử lý theo trạng thái.
- **AC-ASG-02-04** WAITING_INFO giữ câu hỏi đang mở; câu trả lời sau đó được thông báo cho người mới.
- **AC-ASG-02-05** Không đổi sang NV khác phòng ban.
- **AC-ASG-02-06** Retry cùng thao tác không gửi lặp thông báo.

**Ví dụ Edge Case**

Đổi người khi SV đang soạn câu trả lời bổ sung.

**Expected Result**

Version cũ bị báo xung đột; SV tải lại, câu hỏi còn mở và có thể trả lời cho NV mới.

**Hiệu ứng dữ liệu**

Chỉ đổi assignee_id, không đổi status.

**Yêu cầu giao diện**

Người hiện tại, người mới, lý do; xác nhận.

**Liên kết thực hiện và kiểm thử**

Module M05; ưu tiên Must. Tiền đề triển khai: FR-ASG-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-ASG-02-01 đến TS-ASG-02-06.



## [FR-ASG-03] Chuyển Ticket sang phòng ban khác

**Mô tả**

Chuyển Ticket tiếp nhận sai phòng ban sang phòng ban phù hợp, xóa phân công cũ và đưa về hàng chờ mới.

**Actor**

QL

**Preconditions**

- Ticket RECEIVED hoặc PROCESSING thuộc phòng ban QL quản lý; không có bổ sung đang mở.
- Phòng ban đích khác phòng ban hiện tại và có loại vấn đề hoạt động.

**Luồng chính**

1. QL chọn phòng ban đích, loại vấn đề của phòng ban đó và nhập lý do.
2. Máy chủ kiểm quyền trên phòng ban nguồn, danh mục đích và version.
3. Trong giao dịch, đổi department_id và category_id, xóa assignee_id, chuyển RECEIVED, ghi sự kiện và thông báo cho SV, NV cũ và QL đích.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| department_id | Khóa bắt buộc | Phòng ban đích hoạt động, khác nguồn. |
| category_id | Khóa bắt buộc | Loại hoạt động thuộc đích. |
| reason | Văn bản bắt buộc | 10 đến 500 ký tự. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket RECEIVED hoặc PROCESSING thuộc phòng ban QL quản lý; không có bổ sung đang mở. Phòng ban đích khác phòng ban hiện tại và có loại vấn đề hoạt động.
- Kết quả bắt buộc: RECEIVED/PROCESSING → RECEIVED, assignee_id null; hạn không được tính lại.
- Quy tắc dùng chung: BR-01, BR-05, BR-06, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- WAITING_INFO: phải nhận câu trả lời trước khi chuyển; không xóa câu hỏi đang mở.
- RESOLVED, REJECTED, CLOSED: từ chối.
- Đích không hợp lệ hoặc không có QL hoạt động: từ chối.

**Acceptance Criteria**

- **AC-ASG-03-01** Chuyển hợp lệ giữ ticket_id, mã, student_id, nội dung gốc, created_at, started_at và due_at.
- **AC-ASG-03-02** Phòng ban và loại đổi đồng thời, assignee_id rỗng, status RECEIVED.
- **AC-ASG-03-03** Lịch sử giữ phòng ban, loại, người cũ và lý do.
- **AC-ASG-03-04** NV cũ và QL chỉ quản lý nguồn mất quyền Ticket sau chuyển; QL đích và hàng chờ đích thấy Ticket.
- **AC-ASG-03-05** QL nguồn được chuyển tới đích hoạt động dù không quản lý đích; không được đọc Ticket sau chuyển nếu không có quyền đích.
- **AC-ASG-03-06** WAITING_INFO bị chặn, không làm mất nội dung trao đổi.
- **AC-ASG-03-07** Retry không chuyển lại hoặc nhân đôi thông báo.

**Ví dụ Edge Case**

QL chuyển Ticket đúng lúc NV cũ gửi kết quả.

**Expected Result**

Chỉ một version được chấp nhận; thao tác còn lại bị xung đột hoặc mất quyền.

**Hiệu ứng dữ liệu**

RECEIVED/PROCESSING → RECEIVED, assignee_id null; hạn không được tính lại.

**Yêu cầu giao diện**

Phòng ban và loại đích, lý do; nhắc người phụ trách cũ sẽ được bỏ.

**Liên kết thực hiện và kiểm thử**

Module M05; ưu tiên Must. Tiền đề triển khai: FR-TKT-01. Workflow: WF-04. Kịch bản: TS-ASG-03-01 đến TS-ASG-03-07.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
