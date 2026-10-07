# M09 Đặc tả Đóng Ticket và phản hồi

## [FR-FDB-01] Sinh viên xác nhận đóng Ticket

**Mô tả**

Xác nhận đã nhận kết quả giải quyết hoặc từ chối và đóng vòng đời Ticket trước khi đánh giá.

**Actor**

SV

**Preconditions**

- Ticket thuộc SV, trạng thái RESOLVED hoặc REJECTED.

**Luồng chính**

1. SV đọc kết quả và chọn Xác nhận đóng Ticket.
2. Hệ thống yêu cầu xác nhận; máy chủ kiểm chủ, trạng thái và version.
3. Chuyển CLOSED, ghi closed_at và TICKET_CLOSED, giữ outcome và ended_at.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| ticket_id | Khóa bắt buộc | Ticket của SV hiện tại. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket thuộc SV, trạng thái RESOLVED hoặc REJECTED.
- Kết quả bắt buộc: RESOLVED/REJECTED → CLOSED; outcome được giữ.
- Quy tắc dùng chung: BR-04, BR-07, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ticket còn mở: không cho đóng.
- Đã CLOSED với thao tác mới: báo Đã đóng, không thêm sự kiện.
- SV chưa muốn đóng: hủy xác nhận, không thay đổi.

**Acceptance Criteria**

- **AC-FDB-01-01** RESOLVED hoặc REJECTED của mình đóng được thành CLOSED với closed_at từ máy chủ.
- **AC-FDB-01-02** outcome, ended_at, resolution hoặc rejection_reason được giữ nguyên.
- **AC-FDB-01-03** Không đóng Ticket của SV khác hoặc còn RECEIVED, PROCESSING, WAITING_INFO.
- **AC-FDB-01-04** Đóng không tự tạo đánh giá và không yêu cầu chọn điểm.
- **AC-FDB-01-05** Retry chỉ có một TICKET_CLOSED và một mốc closed_at.
- **AC-FDB-01-06** CLOSED không mở lại hoặc tiếp tục xử lý trong phiên bản này.

**Ví dụ Edge Case**

SV đóng một Ticket REJECTED.

**Expected Result**

CLOSED, outcome vẫn REJECTED; báo cáo giải quyết thành công không tính Ticket này.

**Hiệu ứng dữ liệu**

RESOLVED/REJECTED → CLOSED; outcome được giữ.

**Yêu cầu giao diện**

Xác nhận đóng sau nội dung kết quả; sau đóng có liên kết Đánh giá.

**Liên kết thực hiện và kiểm thử**

Module M09; ưu tiên Must. Tiền đề triển khai: FR-OPS-04, FR-OPS-05, FR-STU-03. Workflow: WF-06. Kịch bản: TS-FDB-01-01 đến TS-FDB-01-06.



## [FR-FDB-02] Sinh viên gửi đánh giá mức hài lòng

**Mô tả**

Lưu một đánh giá duy nhất của sinh viên sau khi Ticket đã đóng, kể cả kết quả từ chối.

**Actor**

SV

**Preconditions**

- Ticket thuộc SV, trạng thái CLOSED, chưa có feedback.

**Luồng chính**

1. SV chọn điểm 1 đến 5, nhập nhận xét tùy chọn và chọn Gửi đánh giá.
2. Máy chủ kiểm chủ, trạng thái, giới hạn và khóa duy nhất ticket_id.
3. Lưu điểm, nhận xét, submitted_at, sự kiện FEEDBACK_SUBMITTED; đánh giá trở thành chỉ đọc.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| rating | Số nguyên bắt buộc | 1 Rất không hài lòng, 2 Không hài lòng, 3 Bình thường, 4 Hài lòng, 5 Rất hài lòng. |
| comment | Chuỗi tùy chọn | Tối đa 1000 ký tự sau trim. |
| version, operation_id | Bắt buộc | Chống thay đổi đồng thời và retry. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket thuộc SV, trạng thái CLOSED, chưa có feedback.
- Kết quả bắt buộc: Thêm feedback và tăng version; không đổi status hoặc outcome.
- Quy tắc dùng chung: BR-07, BR-09, BR-10. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Điểm thiếu, ngoài 1 đến 5 hoặc thập phân: không lưu.
- Đã có đánh giá với operation_id mới: báo Đã gửi đánh giá, không ghi đè.
- Chưa CLOSED: hướng dẫn xác nhận đóng trước.

**Acceptance Criteria**

- **AC-FDB-02-01** Điểm 1 hoặc 5 được lưu đúng Ticket, SV và thời điểm; trạng thái giữ CLOSED.
- **AC-FDB-02-02** 0, 6, 2.5, điểm thiếu và nhận xét 1001 ký tự bị từ chối.
- **AC-FDB-02-03** Không đánh giá trước CLOSED hoặc Ticket của SV khác.
- **AC-FDB-02-04** Retry cùng thao tác trả đánh giá cũ; hai thao tác khác chỉ lưu một đánh giá.
- **AC-FDB-02-05** Sau gửi không sửa hoặc xóa feedback trong giao diện nghiệp vụ.
- **AC-FDB-02-06** Ticket có outcome REJECTED vẫn được đánh giá.

**Ví dụ Edge Case**

SV gửi điểm 5 rồi gửi lần hai điểm 1.

**Expected Result**

Điểm đầu tiên giữ nguyên; không ghi đè hoặc tạo bản thứ hai.

**Hiệu ứng dữ liệu**

Thêm feedback và tăng version; không đổi status hoặc outcome.

**Yêu cầu giao diện**

Thang điểm có nhãn đầy đủ, ô nhận xét, nút Gửi; sau gửi chỉ đọc.

**Liên kết thực hiện và kiểm thử**

Module M09; ưu tiên Must. Tiền đề triển khai: FR-FDB-01. Workflow: WF-06. Kịch bản: TS-FDB-02-01 đến TS-FDB-02-06.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
