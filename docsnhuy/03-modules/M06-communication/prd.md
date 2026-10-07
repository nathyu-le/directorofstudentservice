# M06 Đặc tả Trao đổi thông tin

## [FR-COM-01] Yêu cầu sinh viên bổ sung thông tin

**Mô tả**

Đặt một yêu cầu bổ sung cụ thể để sinh viên trả lời trên chính Ticket đang xử lý.

**Actor**

NV

**Preconditions**

- Ticket PROCESSING được giao cho NV và không có câu hỏi bổ sung đang mở.

**Luồng chính**

1. NV nhập nội dung cần bổ sung và chọn Gửi yêu cầu bổ sung.
2. Máy chủ kiểm quyền, trạng thái, version và không còn câu hỏi mở.
3. Tạo information_request OPEN, chuyển WAITING_INFO, ghi sự kiện và thông báo SV.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| content | Văn bản bắt buộc | 10 đến 2000 ký tự. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket PROCESSING được giao cho NV và không có câu hỏi bổ sung đang mở.
- Kết quả bắt buộc: PROCESSING → WAITING_INFO.
- Quy tắc dùng chung: BR-04, BR-07, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Trống hoặc quá dài: không tạo câu hỏi.
- Đã có câu hỏi mở: không tạo câu hỏi thứ hai.
- NV không còn phụ trách: từ chối.

**Acceptance Criteria**

- **AC-COM-01-01** Gửi hợp lệ tạo đúng một câu hỏi OPEN và chuyển PROCESSING sang WAITING_INFO.
- **AC-COM-01-02** Giữ người phụ trách, phòng ban và due_at; SLA tiếp tục tính thời gian.
- **AC-COM-01-03** SV thấy câu hỏi và nút Trả lời.
- **AC-COM-01-04** Khi WAITING_INFO không thể gửi câu hỏi thứ hai, ghi tiến độ, hoàn tất hoặc từ chối.
- **AC-COM-01-05** Retry cùng thao tác chỉ có một câu hỏi, sự kiện và thông báo.

**Ví dụ Edge Case**

NV bấm Gửi yêu cầu bổ sung nhiều lần.

**Expected Result**

Một câu hỏi OPEN và một lần chuyển WAITING_INFO.

**Hiệu ứng dữ liệu**

PROCESSING → WAITING_INFO.

**Yêu cầu giao diện**

Nội dung cần bổ sung; giải thích Ticket sẽ chờ SV trả lời.

**Liên kết thực hiện và kiểm thử**

Module M06; ưu tiên Must. Tiền đề triển khai: FR-OPS-02. Workflow: WF-03. Kịch bản: TS-COM-01-01 đến TS-COM-01-05.



## [FR-COM-02] Sinh viên trả lời yêu cầu bổ sung

**Mô tả**

Trả lời câu hỏi bổ sung đang mở; giữ cùng Ticket và đưa về xử lý.

**Actor**

SV

**Preconditions**

- Ticket thuộc SV, trạng thái WAITING_INFO; information_request_id đang OPEN.

**Luồng chính**

1. SV mở câu hỏi và nhập câu trả lời.
2. Máy chủ kiểm chủ Ticket, câu hỏi mở, version và nội dung.
3. Lưu câu trả lời, đóng câu hỏi bằng ANSWERED, chuyển PROCESSING, ghi sự kiện và thông báo cho NV hiện tại.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| information_request_id | Khóa bắt buộc | Câu hỏi OPEN của Ticket. |
| content | Văn bản bắt buộc | 1 đến 2000 ký tự. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket thuộc SV, trạng thái WAITING_INFO; information_request_id đang OPEN.
- Kết quả bắt buộc: WAITING_INFO → PROCESSING; không tạo Ticket mới.
- Quy tắc dùng chung: BR-04, BR-07, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Sai chủ hoặc câu hỏi thuộc Ticket khác: từ chối.
- Câu hỏi đã trả lời: không tạo câu trả lời mới.
- Version cũ: giữ nội dung form, cho tải lại để kiểm tra.

**Acceptance Criteria**

- **AC-COM-02-01** Câu trả lời hợp lệ tạo một answer, câu hỏi ANSWERED, status PROCESSING.
- **AC-COM-02-02** Giữ ticket_id, student_id, assignee_id, department_id và due_at.
- **AC-COM-02-03** Nội dung trống, chỉ khoảng trắng hoặc 2001 ký tự bị từ chối.
- **AC-COM-02-04** Gửi lặp cùng operation_id không thêm câu trả lời hay sự kiện.
- **AC-COM-02-05** Sau đổi người, câu trả lời được thông báo cho NV mới.
- **AC-COM-02-06** Một câu hỏi chỉ có một câu trả lời hoàn tất; muốn bổ sung tiếp NV phải tạo câu hỏi mới khi PROCESSING.

**Ví dụ Edge Case**

SV trả lời cùng câu hỏi từ hai tab với hai operation_id khác nhau.

**Expected Result**

Chỉ một câu trả lời được lưu; tab còn lại nhận xung đột.

**Hiệu ứng dữ liệu**

WAITING_INFO → PROCESSING; không tạo Ticket mới.

**Yêu cầu giao diện**

Câu hỏi đang mở, ô trả lời, nút Gửi bổ sung.

**Liên kết thực hiện và kiểm thử**

Module M06; ưu tiên Must. Tiền đề triển khai: FR-COM-01. Workflow: WF-03. Kịch bản: TS-COM-02-01 đến TS-COM-02-06.



## [FR-COM-03] Xem hội thoại bổ sung của Ticket

**Mô tả**

Đọc toàn bộ các vòng hỏi và trả lời bổ sung; không phải chat tự do hoặc ghi chú nội bộ.

**Actor**

SV, NV, QL

**Preconditions**

- Có quyền đọc Ticket hiện hành.

**Luồng chính**

1. Người dùng chọn Hội thoại.
2. Máy chủ lấy câu hỏi và câu trả lời theo requested_at tăng dần rồi information_request_id tăng dần.
3. Ghép mỗi câu trả lời với câu hỏi gốc, ghi người gửi, thời điểm và trạng thái câu hỏi.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | Theo quyền đọc Ticket. |
| conversation | Chỉ đọc | Câu hỏi OPEN hoặc ANSWERED và answer tương ứng. |

**Business Rules**

- Điều kiện nghiệp vụ: Có quyền đọc Ticket hiện hành.
- Kết quả bắt buộc: Chỉ đọc; không ghi thêm tin nhắn tự do.
- Quy tắc dùng chung: BR-01, BR-07, BR-12. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Chưa có trao đổi: Chưa có yêu cầu bổ sung.
- Ngoài quyền: không trả nội dung.

**Acceptance Criteria**

- **AC-COM-03-01** Nhiều vòng bổ sung được hiển thị đúng thứ tự và đúng quan hệ câu hỏi trả lời.
- **AC-COM-03-02** Câu hỏi OPEN hiển thị Chờ trả lời; chỉ SV chủ Ticket có liên kết trả lời.
- **AC-COM-03-03** QL và NV được đọc theo quyền nhưng không trả lời thay SV.
- **AC-COM-03-04** Nội dung không bị sửa hoặc mất sau phân công lại hoặc chuyển phòng ban.
- **AC-COM-03-05** HTML trong câu hỏi hoặc câu trả lời không thực thi.

**Ví dụ Edge Case**

Ticket có ba vòng hỏi trả lời rồi đổi người phụ trách.

**Expected Result**

NV mới đọc được cả ba vòng; NV cũ mất quyền đọc.

**Hiệu ứng dữ liệu**

Chỉ đọc; không ghi thêm tin nhắn tự do.

**Yêu cầu giao diện**

Các cặp câu hỏi trả lời; trạng thái câu hỏi đang mở rõ ràng.

**Liên kết thực hiện và kiểm thử**

Module M06; ưu tiên Must. Tiền đề triển khai: FR-COM-01, FR-IAM-03. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-COM-03-01 đến TS-COM-03-05.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
