# Quy ước mã và liên kết giữa ba tài liệu

| Đối tượng | Quy ước | Ví dụ |
| --- | --- | --- |
| Module | M01 đến M10 | M02 Yêu cầu của sinh viên |
| Chức năng | FR + nhóm hành vi + hai số | FR-STU-02 Sinh viên gửi yêu cầu hỗ trợ |
| Business rule | BR + hai số | BR-10 Chống gửi trùng |
| Chuyển trạng thái | ST + hai số | ST-04 PROCESSING sang WAITING_INFO |
| Workflow | WF + hai số | WF-03 Yêu cầu và trả lời bổ sung |
| Acceptance criterion | AC + hậu tố FR + hai số | AC-STU-02-05 |
| Test scenario | TS + hậu tố FR + hai số | TS-STU-02-05 |
| Test đầu cuối | E2E + hai số | E2E-06 |
| Task Master Plan | Mã FR + phần việc | FR-STU-02-BE, FR-STU-02-FE, FR-STU-02-QA |
| Bug | BUG + hậu tố FR + số | BUG-STU-02-001 |
| NFR task | Mã NFR + phần việc | NFR-SEC-02-QA |
| Quyết định kiến trúc | ADR + ba số | ADR-004 |


Mã FR được giữ ổn định sau baseline; đổi tên phải cập nhật mọi nơi dùng, còn bỏ chức năng thì đánh dấu ngừng thay vì cấp lại mã cho hành vi khác. Team Charter quy định quy trình PM chốt FR, BE/FE triển khai và QA đối chiếu AC, báo lỗi, retest, review và bàn giao. Master Plan nhập danh mục từ requirements.csv, dùng đúng feature_id, feature_name, module, priority, dependencies và AC của bản này.

Quy ước task mô tả cách nối yêu cầu vào kế hoạch, không là lịch đã được lập. Mốc Done của task QA phải dựa vào AC và bằng chứng; Done của cả FR còn cần tích hợp, review và không có lỗi chặn. Việc một FR chia thành nhiều task không tăng số chức năng.

[Về danh mục PRD](../README.md)
