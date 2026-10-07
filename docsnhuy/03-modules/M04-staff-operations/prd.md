# M04 Đặc tả Nghiệp vụ xử lý của nhân viên

## [FR-OPS-01] Nhận Ticket từ hàng chờ

**Mô tả**

Nhân viên tự nhận một Ticket chưa phân công của phòng ban mình và bắt đầu xử lý trong cùng thao tác.

**Actor**

NV

**Preconditions**

- Ticket RECEIVED, chưa phân công, thuộc phòng ban của NV; NV hoạt động.

**Luồng chính**

1. NV chọn Nhận Ticket từ hàng chờ.
2. Máy chủ kiểm quyền và điều kiện nhận bằng cập nhật có điều kiện trong giao dịch.
3. Gán assignee_id cho NV, chuyển PROCESSING, ghi started_at lần đầu, tăng version và ghi sự kiện TICKET_CLAIMED.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | RECEIVED và assignee_id rỗng. |
| version, operation_id | Bắt buộc | Chống nhận đồng thời và retry. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket RECEIVED, chưa phân công, thuộc phòng ban của NV; NV hoạt động.
- Kết quả bắt buộc: RECEIVED → PROCESSING; assignee_id được điền.
- Quy tắc dùng chung: BR-02, BR-04, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Đã có người nhận hoặc được QL giao trước: báo Ticket đã được phân công, tải lại hàng chờ.
- Khác phòng ban hoặc sai trạng thái: từ chối.

**Acceptance Criteria**

- **AC-OPS-01-01** Nhận hợp lệ gán đúng NV trong phiên, chuyển PROCESSING và ghi started_at.
- **AC-OPS-01-02** Hai NV nhận đồng thời: đúng một người thành công, một người nhận xung đột.
- **AC-OPS-01-03** NV không nhận Ticket đã phân công cho người khác hoặc ở phòng ban khác.
- **AC-OPS-01-04** Retry của người đã nhận trả kết quả cũ; không thêm lịch sử hay thay started_at.
- **AC-OPS-01-05** NV thắng có quyền xử lý, NV còn lại không có quyền đọc nội dung đã được phân công.

**Ví dụ Edge Case**

QL phân công và NV nhận cùng thời điểm.

**Expected Result**

Chỉ một giao dịch thắng; Ticket luôn có tối đa một người phụ trách.

**Hiệu ứng dữ liệu**

RECEIVED → PROCESSING; assignee_id được điền.

**Yêu cầu giao diện**

Nút Nhận trên dòng chưa phân công; báo kết quả và mở chi tiết.

**Liên kết thực hiện và kiểm thử**

Module M04; ưu tiên Must. Tiền đề triển khai: FR-TKT-01. Workflow: WF-02. Kịch bản: TS-OPS-01-01 đến TS-OPS-01-05.



## [FR-OPS-02] Bắt đầu xử lý Ticket được phân công

**Mô tả**

Bắt đầu Ticket đã được QL giao; khác với tự nhận Ticket chưa phân công.

**Actor**

NV

**Preconditions**

- Ticket RECEIVED và assignee_id là NV trong phiên.

**Luồng chính**

1. NV mở Công việc của tôi và chọn Bắt đầu xử lý.
2. Máy chủ kiểm người phụ trách và version.
3. Chuyển PROCESSING, ghi started_at nếu chưa có và sự kiện PROCESSING_STARTED.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | Đã giao đúng NV. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket RECEIVED và assignee_id là NV trong phiên.
- Kết quả bắt buộc: RECEIVED → PROCESSING; không đổi assignee_id hoặc due_at.
- Quy tắc dùng chung: BR-02, BR-04, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Chưa được giao, đã đổi người hoặc sai trạng thái: từ chối.
- Ticket từng được xử lý trước chuyển phòng ban: giữ started_at đầu tiên.

**Acceptance Criteria**

- **AC-OPS-02-01** NV được giao bắt đầu hợp lệ, status thành PROCESSING.
- **AC-OPS-02-02** Ticket chưa có started_at được ghi thời điểm máy chủ.
- **AC-OPS-02-03** Ticket đã có started_at giữ mốc đầu tiên sau chuyển phòng ban.
- **AC-OPS-02-04** QL hoặc NV khác không bắt đầu thay người được giao.
- **AC-OPS-02-05** Gửi lặp cùng operation_id chỉ tạo một sự kiện và một lần chuyển trạng thái.

**Ví dụ Edge Case**

Ticket được chuyển phòng ban rồi phân công lại.

**Expected Result**

Bắt đầu ở phòng ban mới giữ mã, created_at và started_at đầu tiên.

**Hiệu ứng dữ liệu**

RECEIVED → PROCESSING; không đổi assignee_id hoặc due_at.

**Yêu cầu giao diện**

Nút Bắt đầu xử lý trên Ticket RECEIVED đã giao cho mình.

**Liên kết thực hiện và kiểm thử**

Module M04; ưu tiên Must. Tiền đề triển khai: FR-ASG-01. Workflow: WF-02, WF-04. Kịch bản: TS-OPS-02-01 đến TS-OPS-02-05.



## [FR-OPS-03] Ghi cập nhật tiến độ xử lý

**Mô tả**

Ghi một cập nhật công khai để sinh viên biết tiến độ; cập nhật không phải kết quả hoàn tất.

**Actor**

NV

**Preconditions**

- Ticket PROCESSING và được giao cho NV hiện tại.

**Luồng chính**

1. NV nhập nội dung cập nhật.
2. Máy chủ kiểm nội dung, quyền và version.
3. Lưu cập nhật tiến độ, sự kiện PROGRESS_ADDED và thông báo cho SV.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| content | Văn bản bắt buộc | 1 đến 3000 ký tự sau trim; toàn bộ nội dung được SV nhìn thấy. |
| version, operation_id | Bắt buộc | Kiểm phiên bản và chống lưu lặp. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket PROCESSING và được giao cho NV hiện tại.
- Kết quả bắt buộc: Thêm progress update; giữ trạng thái.
- Quy tắc dùng chung: BR-02, BR-08, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Trống hoặc quá dài: không ghi.
- WAITING_INFO hoặc kết thúc: không ghi tiến độ; dùng đúng thao tác nghiệp vụ.

**Acceptance Criteria**

- **AC-OPS-03-01** Nội dung hợp lệ lưu đúng Ticket và actor, giữ PROCESSING và hạn.
- **AC-OPS-03-02** SV thấy nội dung khi tải lại chi tiết.
- **AC-OPS-03-03** Retry cùng thao tác không thêm cập nhật hoặc thông báo.
- **AC-OPS-03-04** Người phụ trách cũ không ghi sau đổi người.
- **AC-OPS-03-05** Nội dung có ký tự HTML hiển thị như văn bản, không thực thi.

**Ví dụ Edge Case**

NV nhập `<script>alert(1)</script>`.

**Expected Result**

Nội dung hiển thị dạng chữ; không chạy mã trong trình duyệt.

**Hiệu ứng dữ liệu**

Thêm progress update; giữ trạng thái.

**Yêu cầu giao diện**

Ô cập nhật có chú thích Sinh viên sẽ thấy nội dung này.

**Liên kết thực hiện và kiểm thử**

Module M04; ưu tiên Must. Tiền đề triển khai: FR-OPS-02. Workflow: WF-02. Kịch bản: TS-OPS-03-01 đến TS-OPS-03-05.



## [FR-OPS-04] Hoàn tất xử lý Ticket

**Mô tả**

Ghi kết quả giải quyết và kết thúc trách nhiệm xử lý của Ticket.

**Actor**

NV

**Preconditions**

- Ticket PROCESSING, giao cho NV hiện tại, không có yêu cầu bổ sung đang mở.

**Luồng chính**

1. NV nhập kết quả và chọn Hoàn tất.
2. Máy chủ kiểm quyền, trạng thái, yêu cầu bổ sung, version và dữ liệu.
3. Lưu resolution, outcome RESOLVED, ended_at, chuyển RESOLVED, ghi sự kiện và thông báo SV xác nhận đóng.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| resolution | Văn bản bắt buộc | 10 đến 5000 ký tự. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket PROCESSING, giao cho NV hiện tại, không có yêu cầu bổ sung đang mở.
- Kết quả bắt buộc: PROCESSING → RESOLVED; outcome giữ cho báo cáo sau đóng.
- Quy tắc dùng chung: BR-04, BR-07, BR-08, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- WAITING_INFO hoặc còn yêu cầu bổ sung mở: từ chối.
- Kết quả trống hoặc không hợp lệ: không kết thúc.
- Version cũ: yêu cầu tải lại trước khi thao tác mới.

**Acceptance Criteria**

- **AC-OPS-04-01** Kết quả hợp lệ chuyển PROCESSING sang RESOLVED, outcome RESOLVED, ended_at theo máy chủ.
- **AC-OPS-04-02** Không tạo closed_at; SV phải xác nhận đóng ở FR-FDB-01.
- **AC-OPS-04-03** Thiếu hoặc dưới 10 ký tự không lưu kết quả hay đổi trạng thái.
- **AC-OPS-04-04** WAITING_INFO và NV khác không hoàn tất.
- **AC-OPS-04-05** Retry cùng mã chỉ có một sự kiện hoàn tất và một thông báo cho SV.
- **AC-OPS-04-06** Sau hoàn tất, các thao tác phân công, tiến độ, bổ sung và đổi hạn đều bị chặn.

**Ví dụ Edge Case**

NV nhấn Hoàn tất lần hai sau khi response đầu bị mất.

**Expected Result**

Trả kết quả của thao tác đầu; ended_at không bị đổi.

**Hiệu ứng dữ liệu**

PROCESSING → RESOLVED; outcome giữ cho báo cáo sau đóng.

**Yêu cầu giao diện**

Kết quả, nút Hoàn tất và xác nhận trước khi kết thúc.

**Liên kết thực hiện và kiểm thử**

Module M04; ưu tiên Must. Tiền đề triển khai: FR-OPS-02. Workflow: WF-05, WF-06. Kịch bản: TS-OPS-04-01 đến TS-OPS-04-06.



## [FR-OPS-05] Từ chối xử lý Ticket

**Mô tả**

Ghi lý do không thể đáp ứng yêu cầu trong phạm vi dịch vụ; từ chối là một kết quả riêng với hoàn tất.

**Actor**

NV

**Preconditions**

- Ticket PROCESSING, được giao cho NV hiện tại, không có bổ sung đang mở.

**Luồng chính**

1. NV chọn Từ chối và nhập lý do.
2. Máy chủ kiểm quyền, trạng thái và nội dung.
3. Chuyển REJECTED, outcome REJECTED, ghi rejection_reason, ended_at, lịch sử và thông báo SV.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| rejection_reason | Văn bản bắt buộc | 10 đến 1000 ký tự; sinh viên được đọc. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket PROCESSING, được giao cho NV hiện tại, không có bổ sung đang mở.
- Kết quả bắt buộc: PROCESSING → REJECTED.
- Quy tắc dùng chung: BR-04, BR-07, BR-08, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Sai phòng ban phụ trách nhưng có phòng ban phù hợp: dùng chuyển phòng ban, không dùng từ chối để thay việc chuyển.
- Thiếu lý do, sai trạng thái hoặc sai người phụ trách: không lưu.

**Acceptance Criteria**

- **AC-OPS-05-01** Từ chối hợp lệ tạo REJECTED, outcome REJECTED, ended_at và lý do đọc được bởi SV.
- **AC-OPS-05-02** Không ghi resolution hoặc closed_at thay cho từ chối.
- **AC-OPS-05-03** Lý do trống, 9 hoặc 1001 ký tự bị từ chối.
- **AC-OPS-05-04** WAITING_INFO không được chuyển trực tiếp REJECTED.
- **AC-OPS-05-05** Retry không thêm sự kiện từ chối hoặc thông báo.
- **AC-OPS-05-06** SV có thể xác nhận đóng và đánh giá Ticket bị từ chối theo WF-06.

**Ví dụ Edge Case**

NV từ chối Ticket trong khi câu hỏi bổ sung đang mở.

**Expected Result**

Bị chặn; Ticket vẫn WAITING_INFO và câu hỏi vẫn mở.

**Hiệu ứng dữ liệu**

PROCESSING → REJECTED.

**Yêu cầu giao diện**

Lý do từ chối; xác nhận có nội dung SV nhìn thấy.

**Liên kết thực hiện và kiểm thử**

Module M04; ưu tiên Must. Tiền đề triển khai: FR-OPS-02. Workflow: WF-05, WF-06. Kịch bản: TS-OPS-05-01 đến TS-OPS-05-06.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
