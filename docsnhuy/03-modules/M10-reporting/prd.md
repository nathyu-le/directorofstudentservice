# M10 Đặc tả Báo cáo

## [FR-RPT-01] Thống kê Ticket theo trạng thái

**Mô tả**

Đếm Ticket theo sáu trạng thái hiện tại.

**Actor**

QL

**Preconditions**

- QL đăng nhập; có phạm vi phòng ban được quản lý.

**Luồng chính**

1. QL chọn khoảng ngày tạo Ticket và phòng ban rồi chọn Xem báo cáo.
2. Máy chủ kiểm bộ lọc, giới hạn quyền, chụp as_of_at và tập Ticket thống nhất.
3. Nhóm theo status; luôn trả đủ RECEIVED, PROCESSING, WAITING_INFO, RESOLVED, REJECTED, CLOSED.
4. Hiển thị giá trị, số mẫu, bộ lọc và mốc báo cáo.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| from_date, to_date | Ngày bắt buộc | YYYY-MM-DD; from_date ≤ to_date; lọc created_at trong ngày Việt Nam. |
| department_id | Khóa tùy chọn | Rỗng là toàn bộ phòng ban QL quản lý; ngoài quyền bị từ chối. |
| as_of_at | Máy chủ tạo | Một mốc và một ảnh chụp nhất quán cho các số liệu cùng lần chạy. |

**Business Rules**

- Điều kiện nghiệp vụ: QL đăng nhập; có phạm vi phòng ban được quản lý.
- Kết quả bắt buộc: Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.
- Quy tắc dùng chung: BR-01, BR-12, BR-15. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngày thiếu, không hợp lệ hoặc đảo ngược: báo lỗi, không chạy.
- Phòng ban ngoài quyền: từ chối.
- Tập rỗng: số đếm 0; trung bình NULL hiển thị Chưa có dữ liệu.

**Acceptance Criteria**

- **AC-RPT-01-01** Tổng sáu nhóm bằng tổng Ticket cùng bộ lọc.
- **AC-RPT-01-02** CLOSED được đếm riêng, không đếm thêm vào RESOLVED hay REJECTED.
- **AC-RPT-01-03** Nhóm không có Ticket trả 0.
- **AC-RPT-01-04** Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.
- **AC-RPT-01-05** Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

**Ví dụ Edge Case**

Một Ticket RESOLVED được SV đóng trước khi chạy lại báo cáo.

**Expected Result**

Nhóm RESOLVED giảm một, CLOSED tăng một; tổng không đổi.

**Hiệu ứng dữ liệu**

Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.

**Yêu cầu giao diện**

Bộ lọc ngày, phòng ban; bảng hoặc biểu đồ chỉ số; số mẫu và as_of_at.

**Liên kết thực hiện và kiểm thử**

Module M10; ưu tiên Must. Tiền đề triển khai: FR-STU-02, FR-FDB-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-RPT-01-01 đến TS-RPT-01-05.



## [FR-RPT-02] Xem danh sách Ticket quá hạn đang mở

**Mô tả**

Liệt kê Ticket đang mở vượt hạn tại mốc báo cáo.

**Actor**

QL

**Preconditions**

- QL đăng nhập; có phạm vi phòng ban được quản lý.

**Luồng chính**

1. QL chọn khoảng ngày tạo Ticket và phòng ban rồi chọn Xem báo cáo.
2. Máy chủ kiểm bộ lọc, giới hạn quyền, chụp as_of_at và tập Ticket thống nhất.
3. Lọc status trong ba trạng thái mở và due_at < as_of_at; sắp due_at tăng dần rồi ticket_id tăng dần.
4. Hiển thị giá trị, số mẫu, bộ lọc và mốc báo cáo.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| from_date, to_date | Ngày bắt buộc | YYYY-MM-DD; from_date ≤ to_date; lọc created_at trong ngày Việt Nam. |
| department_id | Khóa tùy chọn | Rỗng là toàn bộ phòng ban QL quản lý; ngoài quyền bị từ chối. |
| as_of_at | Máy chủ tạo | Một mốc và một ảnh chụp nhất quán cho các số liệu cùng lần chạy. |

**Business Rules**

- Điều kiện nghiệp vụ: QL đăng nhập; có phạm vi phòng ban được quản lý.
- Kết quả bắt buộc: Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.
- Quy tắc dùng chung: BR-01, BR-12, BR-15. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngày thiếu, không hợp lệ hoặc đảo ngược: báo lỗi, không chạy.
- Phòng ban ngoài quyền: từ chối.
- Tập rỗng: số đếm 0; trung bình NULL hiển thị Chưa có dữ liệu.

**Acceptance Criteria**

- **AC-RPT-02-01** Ticket đúng hạn tại as_of_at không vào danh sách.
- **AC-RPT-02-02** WAITING_INFO vượt hạn được tính; Ticket kết thúc không được tính.
- **AC-RPT-02-03** Danh sách và số quá hạn dùng cùng as_of_at và tổng bằng số dòng đủ điều kiện.
- **AC-RPT-02-04** Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.
- **AC-RPT-02-05** Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

**Ví dụ Edge Case**

Gia hạn Ticket rồi chạy lại báo cáo trước hạn mới.

**Expected Result**

Ticket rời danh sách quá hạn; lịch sử hạn cũ vẫn giữ.

**Hiệu ứng dữ liệu**

Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.

**Yêu cầu giao diện**

Bộ lọc ngày, phòng ban; bảng hoặc biểu đồ chỉ số; số mẫu và as_of_at.

**Liên kết thực hiện và kiểm thử**

Module M10; ưu tiên Must. Tiền đề triển khai: FR-SLA-03. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-RPT-02-01 đến TS-RPT-02-05.



## [FR-RPT-03] Tính thời gian giải quyết thành công trung bình

**Mô tả**

Tính thời gian từ tạo đến kết quả giải quyết thành công, gồm thời gian chờ sinh viên.

**Actor**

QL

**Preconditions**

- QL đăng nhập; có phạm vi phòng ban được quản lý.

**Luồng chính**

1. QL chọn khoảng ngày tạo Ticket và phòng ban rồi chọn Xem báo cáo.
2. Máy chủ kiểm bộ lọc, giới hạn quyền, chụp as_of_at và tập Ticket thống nhất.
3. Chỉ lấy outcome RESOLVED và ended_at hợp lệ, kể cả đã CLOSED; tính AVG((ended_at-created_at)/3600) giờ.
4. Hiển thị giá trị, số mẫu, bộ lọc và mốc báo cáo.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| from_date, to_date | Ngày bắt buộc | YYYY-MM-DD; from_date ≤ to_date; lọc created_at trong ngày Việt Nam. |
| department_id | Khóa tùy chọn | Rỗng là toàn bộ phòng ban QL quản lý; ngoài quyền bị từ chối. |
| as_of_at | Máy chủ tạo | Một mốc và một ảnh chụp nhất quán cho các số liệu cùng lần chạy. |

**Business Rules**

- Điều kiện nghiệp vụ: QL đăng nhập; có phạm vi phòng ban được quản lý.
- Kết quả bắt buộc: Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.
- Quy tắc dùng chung: BR-01, BR-12, BR-15. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngày thiếu, không hợp lệ hoặc đảo ngược: báo lỗi, không chạy.
- Phòng ban ngoài quyền: từ chối.
- Tập rỗng: số đếm 0; trung bình NULL hiển thị Chưa có dữ liệu.

**Acceptance Criteria**

- **AC-RPT-03-01** Ticket thành công sau 2 giờ và 4 giờ cho trung bình 3.0 giờ, mẫu 2.
- **AC-RPT-03-02** CLOSED với outcome RESOLVED vẫn được tính; outcome REJECTED không được tính.
- **AC-RPT-03-03** Không có mẫu hợp lệ trả NULL, không trả 0 giờ.
- **AC-RPT-03-04** Không dùng closed_at thay ended_at và không trừ thời gian WAITING_INFO.
- **AC-RPT-03-05** Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.
- **AC-RPT-03-06** Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

**Ví dụ Edge Case**

Ticket giải quyết sau 4 giờ nhưng sinh viên đóng sau 2 ngày.

**Expected Result**

Thời gian giải quyết vẫn 4.0 giờ.

**Hiệu ứng dữ liệu**

Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.

**Yêu cầu giao diện**

Bộ lọc ngày, phòng ban; bảng hoặc biểu đồ chỉ số; số mẫu và as_of_at.

**Liên kết thực hiện và kiểm thử**

Module M10; ưu tiên Must. Tiền đề triển khai: FR-OPS-04, FR-FDB-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-RPT-03-01 đến TS-RPT-03-06.



## [FR-RPT-04] Thống kê Ticket theo loại vấn đề

**Mô tả**

Đếm Ticket theo loại vấn đề hiện tại để biết nhóm yêu cầu phổ biến.

**Actor**

QL

**Preconditions**

- QL đăng nhập; có phạm vi phòng ban được quản lý.

**Luồng chính**

1. QL chọn khoảng ngày tạo Ticket và phòng ban rồi chọn Xem báo cáo.
2. Máy chủ kiểm bộ lọc, giới hạn quyền, chụp as_of_at và tập Ticket thống nhất.
3. Nhóm theo category_id hiện tại; sắp count giảm dần rồi category_id tăng dần.
4. Hiển thị giá trị, số mẫu, bộ lọc và mốc báo cáo.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| from_date, to_date | Ngày bắt buộc | YYYY-MM-DD; from_date ≤ to_date; lọc created_at trong ngày Việt Nam. |
| department_id | Khóa tùy chọn | Rỗng là toàn bộ phòng ban QL quản lý; ngoài quyền bị từ chối. |
| as_of_at | Máy chủ tạo | Một mốc và một ảnh chụp nhất quán cho các số liệu cùng lần chạy. |

**Business Rules**

- Điều kiện nghiệp vụ: QL đăng nhập; có phạm vi phòng ban được quản lý.
- Kết quả bắt buộc: Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.
- Quy tắc dùng chung: BR-01, BR-12, BR-15. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngày thiếu, không hợp lệ hoặc đảo ngược: báo lỗi, không chạy.
- Phòng ban ngoài quyền: từ chối.
- Tập rỗng: số đếm 0; trung bình NULL hiển thị Chưa có dữ liệu.

**Acceptance Criteria**

- **AC-RPT-04-01** Tổng các loại bằng tổng Ticket trong bộ lọc.
- **AC-RPT-04-02** Phân loại lại hoặc chuyển phòng ban dùng loại hiện tại và phạm vi hiện tại.
- **AC-RPT-04-03** Loại hiện không có Ticket có thể ẩn; tập rỗng hiển thị Không có dữ liệu.
- **AC-RPT-04-04** Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.
- **AC-RPT-04-05** Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

**Ví dụ Edge Case**

Ticket đổi loại sau ngày tạo nhưng vẫn nằm trong khoảng lọc ngày tạo.

**Expected Result**

Đếm tại loại mới, không đếm lặp loại cũ.

**Hiệu ứng dữ liệu**

Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.

**Yêu cầu giao diện**

Bộ lọc ngày, phòng ban; bảng hoặc biểu đồ chỉ số; số mẫu và as_of_at.

**Liên kết thực hiện và kiểm thử**

Module M10; ưu tiên Must. Tiền đề triển khai: FR-TKT-02, FR-ASG-03. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-RPT-04-01 đến TS-RPT-04-05.



## [FR-RPT-05] Thống kê tải công việc theo nhân viên

**Mô tả**

Đếm Ticket đang mở theo người phụ trách hiện tại, gồm nhóm chưa phân công.

**Actor**

QL

**Preconditions**

- QL đăng nhập; có phạm vi phòng ban được quản lý.

**Luồng chính**

1. QL chọn khoảng ngày tạo Ticket và phòng ban rồi chọn Xem báo cáo.
2. Máy chủ kiểm bộ lọc, giới hạn quyền, chụp as_of_at và tập Ticket thống nhất.
3. Lấy ba trạng thái mở, nhóm theo assignee_id; null thành Chưa phân công. NV hoạt động trong phạm vi vẫn hiện 0 khi không có việc.
4. Hiển thị giá trị, số mẫu, bộ lọc và mốc báo cáo.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| from_date, to_date | Ngày bắt buộc | YYYY-MM-DD; from_date ≤ to_date; lọc created_at trong ngày Việt Nam. |
| department_id | Khóa tùy chọn | Rỗng là toàn bộ phòng ban QL quản lý; ngoài quyền bị từ chối. |
| as_of_at | Máy chủ tạo | Một mốc và một ảnh chụp nhất quán cho các số liệu cùng lần chạy. |

**Business Rules**

- Điều kiện nghiệp vụ: QL đăng nhập; có phạm vi phòng ban được quản lý.
- Kết quả bắt buộc: Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.
- Quy tắc dùng chung: BR-01, BR-12, BR-15. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngày thiếu, không hợp lệ hoặc đảo ngược: báo lỗi, không chạy.
- Phòng ban ngoài quyền: từ chối.
- Tập rỗng: số đếm 0; trung bình NULL hiển thị Chưa có dữ liệu.

**Acceptance Criteria**

- **AC-RPT-05-01** Tổng nhóm bằng số Ticket mở cùng bộ lọc.
- **AC-RPT-05-02** RECEIVED đã phân công, PROCESSING và WAITING_INFO tính vào người hiện tại.
- **AC-RPT-05-03** RESOLVED, REJECTED, CLOSED không tính tải mở.
- **AC-RPT-05-04** Đổi người chuyển đúng một Ticket từ nhóm cũ sang nhóm mới; chuyển phòng ban đưa về Chưa phân công ở đích.
- **AC-RPT-05-05** Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.
- **AC-RPT-05-06** Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

**Ví dụ Edge Case**

Nhân viên không có Ticket mở trong khoảng lọc.

**Expected Result**

Hiện 0; không tính Ticket đã kết thúc vào tải mở.

**Hiệu ứng dữ liệu**

Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.

**Yêu cầu giao diện**

Bộ lọc ngày, phòng ban; bảng hoặc biểu đồ chỉ số; số mẫu và as_of_at.

**Liên kết thực hiện và kiểm thử**

Module M10; ưu tiên Must. Tiền đề triển khai: FR-ASG-01, FR-ASG-02, FR-ASG-03. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-RPT-05-01 đến TS-RPT-05-06.



## [FR-RPT-06] Thống kê mức hài lòng của sinh viên

**Mô tả**

Đếm đánh giá theo điểm, tính điểm trung bình và tỷ lệ phản hồi sau đóng.

**Actor**

QL

**Preconditions**

- QL đăng nhập; có phạm vi phòng ban được quản lý.

**Luồng chính**

1. QL chọn khoảng ngày tạo Ticket và phòng ban rồi chọn Xem báo cáo.
2. Máy chủ kiểm bộ lọc, giới hạn quyền, chụp as_of_at và tập Ticket thống nhất.
3. Lấy feedback của Ticket trong tập lọc ngày tạo; AVG(rating), số đánh giá, số CLOSED và tỷ lệ số đánh giá/số CLOSED ×100.
4. Hiển thị giá trị, số mẫu, bộ lọc và mốc báo cáo.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| from_date, to_date | Ngày bắt buộc | YYYY-MM-DD; from_date ≤ to_date; lọc created_at trong ngày Việt Nam. |
| department_id | Khóa tùy chọn | Rỗng là toàn bộ phòng ban QL quản lý; ngoài quyền bị từ chối. |
| as_of_at | Máy chủ tạo | Một mốc và một ảnh chụp nhất quán cho các số liệu cùng lần chạy. |

**Business Rules**

- Điều kiện nghiệp vụ: QL đăng nhập; có phạm vi phòng ban được quản lý.
- Kết quả bắt buộc: Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.
- Quy tắc dùng chung: BR-01, BR-12, BR-15. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Ngày thiếu, không hợp lệ hoặc đảo ngược: báo lỗi, không chạy.
- Phòng ban ngoài quyền: từ chối.
- Tập rỗng: số đếm 0; trung bình NULL hiển thị Chưa có dữ liệu.

**Acceptance Criteria**

- **AC-RPT-06-01** Điểm 3 và 5 cho trung bình 4.0, số mẫu 2.
- **AC-RPT-06-02** Ticket CLOSED chưa đánh giá không được coi là điểm 0.
- **AC-RPT-06-03** Phân phối đủ các điểm 1 đến 5; tổng bằng số đánh giá.
- **AC-RPT-06-04** Không có CLOSED: tỷ lệ NULL; có CLOSED nhưng chưa feedback: tỷ lệ 0.0%.
- **AC-RPT-06-05** Đánh giá Ticket outcome REJECTED được tính, không lọc bỏ.
- **AC-RPT-06-06** Một Ticket chỉ được đếm một lần dù có nhiều sự kiện hoặc vòng bổ sung.
- **AC-RPT-06-07** Bộ lọc và dữ liệu ngoài phạm vi được kiểm ở máy chủ.

**Ví dụ Edge Case**

Ba Ticket CLOSED trong tập ngày tạo, chỉ hai Ticket có điểm 3 và 5.

**Expected Result**

Trung bình 4.0, mẫu 2, tỷ lệ phản hồi 66.7%.

**Hiệu ứng dữ liệu**

Chỉ đọc; cùng định nghĩa dữ liệu ở phần Báo cáo.

**Yêu cầu giao diện**

Bộ lọc ngày, phòng ban; bảng hoặc biểu đồ chỉ số; số mẫu và as_of_at.

**Liên kết thực hiện và kiểm thử**

Module M10; ưu tiên Must. Tiền đề triển khai: FR-FDB-02. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-RPT-06-01 đến TS-RPT-06-07.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
