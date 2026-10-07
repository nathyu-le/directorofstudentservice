# M08 Đặc tả Hạn xử lý

## [FR-SLA-01] Tính hạn xử lý khi tạo Ticket

**Mô tả**

Chụp thời lượng SLA của loại vấn đề và tạo due_at ngay khi tạo Ticket.

**Actor**

Hệ thống

**Preconditions**

- Loại vấn đề hoạt động có sla_hours hợp lệ trong dữ liệu nạp sẵn.

**Luồng chính**

1. Máy chủ đọc sla_hours của loại tại thời điểm tạo.
2. Lưu sla_hours_snapshot và tính due_at = created_at + sla_hours giờ liên tục.
3. Lưu cùng giao dịch tạo Ticket; nếu thiếu cấu hình không tạo Ticket.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| sla_hours | Số nguyên cấu hình | 1 đến 168 giờ; bộ mẫu dùng 48 giờ. |
| created_at | Máy chủ xác định | Lưu UTC, hiển thị giờ Việt Nam. |
| due_at | Hệ thống tính | Cộng giờ liên tục, gồm cuối tuần và thời gian chờ. |

**Business Rules**

- Điều kiện nghiệp vụ: Loại vấn đề hoạt động có sla_hours hợp lệ trong dữ liệu nạp sẵn.
- Kết quả bắt buộc: Tạo due_at và sla_hours_snapshot; không có trạng thái SLA riêng.
- Quy tắc dùng chung: BR-03, BR-14. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- SLA thiếu, bằng 0 hoặc vượt 168: từ chối tạo và ghi lỗi cấu hình, không để Ticket thiếu hạn.

**Acceptance Criteria**

- **AC-SLA-01-01** SLA 48 giờ và created_at 2026-10-12 09:00 giờ Việt Nam cho due_at 2026-10-14 09:00.
- **AC-SLA-01-02** Hạn và snapshot được lưu trong cùng giao dịch tạo Ticket.
- **AC-SLA-01-03** Cuối tuần và WAITING_INFO không dừng đồng hồ SLA.
- **AC-SLA-01-04** Đổi loại, phân công lại hoặc chuyển phòng ban không tự tính lại due_at.
- **AC-SLA-01-05** SLA cấu hình không hợp lệ ngăn tạo Ticket, không để dữ liệu thiếu hạn.

**Ví dụ Edge Case**

Ticket tạo vào thứ Sáu 16:00 với SLA 48 giờ.

**Expected Result**

Hạn là Chủ nhật 16:00; không tự dịch sang thứ Hai.

**Hiệu ứng dữ liệu**

Tạo due_at và sla_hours_snapshot; không có trạng thái SLA riêng.

**Yêu cầu giao diện**

Chi tiết Ticket hiển thị Hạn xử lý theo thời điểm lịch.

**Liên kết thực hiện và kiểm thử**

Module M08; ưu tiên Must. Tiền đề triển khai: FR-STU-02. Workflow: WF-01. Kịch bản: TS-SLA-01-01 đến TS-SLA-01-05.



## [FR-SLA-02] Điều chỉnh hạn xử lý Ticket

**Mô tả**

Đổi hạn của một Ticket đang mở bằng quyết định có lý do và lịch sử.

**Actor**

QL

**Preconditions**

- Ticket RECEIVED, PROCESSING hoặc WAITING_INFO thuộc phạm vi QL.

**Luồng chính**

1. QL chọn hạn mới và nhập lý do.
2. Máy chủ kiểm thời điểm, quyền và version.
3. Lưu due_at mới, giữ snapshot SLA ban đầu, ghi DUE_DATE_CHANGED và thông báo SV, NV hiện tại.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| due_at | Thời điểm bắt buộc | Theo giờ Việt Nam trên UI; không trước created_at hoặc trước thời điểm lưu. |
| reason | Văn bản bắt buộc | 10 đến 500 ký tự. |
| version, operation_id | Bắt buộc | Theo quy tắc thao tác ghi. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket RECEIVED, PROCESSING hoặc WAITING_INFO thuộc phạm vi QL.
- Kết quả bắt buộc: Chỉ đổi due_at, không đổi trạng thái hay started_at.
- Quy tắc dùng chung: BR-05, BR-09, BR-10, BR-14. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Hạn trong quá khứ: từ chối.
- Hạn giống hiện tại: Không có thay đổi, không ghi sự kiện.
- Ticket đã kết thúc: không điều chỉnh hạn.

**Acceptance Criteria**

- **AC-SLA-02-01** Đổi hợp lệ lưu đúng thời điểm và lý do, giữ status và sla_hours_snapshot.
- **AC-SLA-02-02** Hạn trước created_at hoặc trước thời điểm máy chủ bị từ chối.
- **AC-SLA-02-03** Mốc đúng thời điểm máy chủ được chấp nhận; tại đúng hạn chưa quá hạn.
- **AC-SLA-02-04** Ticket kết thúc hoặc ngoài phạm vi bị chặn.
- **AC-SLA-02-05** Lịch sử ghi hạn cũ và mới; báo cáo sau tải lại dùng hạn mới.
- **AC-SLA-02-06** Gia hạn Ticket đang quá hạn chỉ làm thay nhãn hiện tại, không xóa lịch sử thay hạn.

**Ví dụ Edge Case**

QL gia hạn một Ticket đã quá hạn đến ngày hôm sau.

**Expected Result**

Nhãn quá hạn hiện tại mất nếu chưa tới hạn mới; lịch sử thay đổi vẫn được giữ.

**Hiệu ứng dữ liệu**

Chỉ đổi due_at, không đổi trạng thái hay started_at.

**Yêu cầu giao diện**

Hạn cũ, chọn ngày giờ mới, lý do.

**Liên kết thực hiện và kiểm thử**

Module M08; ưu tiên Must. Tiền đề triển khai: FR-SLA-01, FR-TKT-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-SLA-02-01 đến TS-SLA-02-06.



## [FR-SLA-03] Xác định và hiển thị Ticket quá hạn

**Mô tả**

Tính cờ quá hạn trên dữ liệu hiện tại và tạo một thông báo khi một hạn cụ thể bị vượt.

**Actor**

Hệ thống

**Preconditions**

- Ticket có due_at; tác vụ quét SLA hoạt động mỗi tối đa 60 giây.

**Luồng chính**

1. Khi đọc danh sách hoặc chi tiết, lấy as_of_at từ máy chủ.
2. Tính is_overdue khi Ticket đang mở và as_of_at > due_at.
3. Tác vụ quét ghi sự kiện OVERDUE_DETECTED duy nhất theo ticket_id và due_at hiện hành; tạo thông báo theo FR-NOT-01.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| open_statuses | Tập cố định | RECEIVED, PROCESSING, WAITING_INFO. |
| as_of_at | Thời điểm hệ thống | Dùng một mốc cho cả kết quả đọc. |
| overdue_key | Khóa chống trùng | ticket_id và due_at hiện hành. |

**Business Rules**

- Điều kiện nghiệp vụ: Ticket có due_at; tác vụ quét SLA hoạt động mỗi tối đa 60 giây.
- Kết quả bắt buộc: Cờ suy ra khi đọc; sự kiện cảnh báo không tăng version nghiệp vụ Ticket.
- Quy tắc dùng chung: BR-04, BR-13, BR-14. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Tác vụ quét lỗi: nhãn vẫn tính đúng lúc đọc; cảnh báo được retry sau phục hồi.
- Ticket kết thúc: không hiện nhãn quá hạn đang mở.

**Acceptance Criteria**

- **AC-SLA-03-01** Tại as_of_at bằng due_at, is_overdue false; vượt một giây thì true với Ticket mở.
- **AC-SLA-03-02** WAITING_INFO vẫn được tính quá hạn nếu vượt hạn.
- **AC-SLA-03-03** RESOLVED, REJECTED, CLOSED không bị đếm quá hạn đang mở.
- **AC-SLA-03-04** Một ticket_id và due_at chỉ có một sự kiện cảnh báo, kể cả quét nhiều lần.
- **AC-SLA-03-05** Gia hạn tạo khóa hạn mới; nếu hạn mới bị vượt có thể có một cảnh báo mới.
- **AC-SLA-03-06** Nhãn không đổi status và khớp danh sách báo cáo quá hạn cùng as_of_at.

**Ví dụ Edge Case**

Tác vụ quét chạy lặp mười lần sau khi vượt hạn.

**Expected Result**

Một sự kiện cảnh báo và một thông báo mỗi người nhận cho hạn đó.

**Hiệu ứng dữ liệu**

Cờ suy ra khi đọc; sự kiện cảnh báo không tăng version nghiệp vụ Ticket.

**Yêu cầu giao diện**

Nhãn Quá hạn cạnh hạn; trạng thái Ticket giữ riêng.

**Liên kết thực hiện và kiểm thử**

Module M08; ưu tiên Must. Tiền đề triển khai: FR-SLA-01, FR-NOT-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-SLA-03-01 đến TS-SLA-03-06.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
