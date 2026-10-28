# Truy vết chức năng, kiểm thử và milestone

Phạm vi đặc tả chức năng giữ nguyên từ repo. Revision kế hoạch 6.0; FR cha không cộng giờ. Mỗi FR có 9 task theo mã bên dưới; mỗi task công việc được chuẩn bị thành một GitHub Issue. Trạng thái áp dụng native GitHub xem APPLY_STATUS.md ở gói bàn giao, không suy từ bảng này.

| FR | Hành vi | Mốc hoàn tất | AC | TC | Đặc tả |
| --- | --- | --- | --- | --- | --- |
| FR-STU-01 | Gửi yêu cầu hỗ trợ | MS-02 | AC-STU-01-01, AC-STU-01-02, AC-STU-01-03, AC-STU-01-04, AC-STU-01-05, AC-STU-01-06, AC-STU-01-07, AC-STU-01-08 | TC-STU-01-01, TC-STU-01-02, TC-STU-01-03, TC-STU-01-04, TC-STU-01-05, TC-STU-01-06, TC-STU-01-07, TC-STU-01-08 | [Đọc](../../docs/03-modules/01-student-support/requirements/FR-STU-01.md) |
| FR-STU-02 | Xem danh sách yêu cầu của tôi | MS-02 | AC-STU-02-01, AC-STU-02-02, AC-STU-02-03, AC-STU-02-04, AC-STU-02-05 | TC-STU-02-01, TC-STU-02-02, TC-STU-02-03, TC-STU-02-04, TC-STU-02-05 | [Đọc](../../docs/03-modules/01-student-support/requirements/FR-STU-02.md) |
| FR-STU-03 | Xem chi tiết và tiến độ yêu cầu | MS-02 | AC-STU-03-01, AC-STU-03-02, AC-STU-03-03, AC-STU-03-04, AC-STU-03-05 | TC-STU-03-01, TC-STU-03-02, TC-STU-03-03, TC-STU-03-04, TC-STU-03-05 | [Đọc](../../docs/03-modules/01-student-support/requirements/FR-STU-03.md) |
| FR-STU-04 | Bổ sung thông tin được yêu cầu | MS-03 | AC-STU-04-01, AC-STU-04-02, AC-STU-04-03, AC-STU-04-04, AC-STU-04-05, AC-STU-04-06, AC-STU-04-07 | TC-STU-04-01, TC-STU-04-02, TC-STU-04-03, TC-STU-04-04, TC-STU-04-05, TC-STU-04-06, TC-STU-04-07 | [Đọc](../../docs/03-modules/01-student-support/requirements/FR-STU-04.md) |
| FR-STU-05 | Phản hồi kết quả hỗ trợ | MS-04 | AC-STU-05-01, AC-STU-05-02, AC-STU-05-03, AC-STU-05-04, AC-STU-05-05, AC-STU-05-06, AC-STU-05-07 | TC-STU-05-01, TC-STU-05-02, TC-STU-05-03, TC-STU-05-04, TC-STU-05-05, TC-STU-05-06, TC-STU-05-07 | [Đọc](../../docs/03-modules/01-student-support/requirements/FR-STU-05.md) |
| FR-DSP-01 | Xem hàng chờ điều phối | MS-02 | AC-DSP-01-01, AC-DSP-01-02, AC-DSP-01-03, AC-DSP-01-04 | TC-DSP-01-01, TC-DSP-01-02, TC-DSP-01-03, TC-DSP-01-04 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-01.md) |
| FR-DSP-02 | Xem hồ sơ xử lý và lịch sử nghiệp vụ | MS-02 | AC-DSP-02-01, AC-DSP-02-02, AC-DSP-02-03, AC-DSP-02-04 | TC-DSP-02-01, TC-DSP-02-02, TC-DSP-02-03, TC-DSP-02-04 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-02.md) |
| FR-DSP-03 | Phân loại yêu cầu | MS-03 | AC-DSP-03-01, AC-DSP-03-02, AC-DSP-03-03, AC-DSP-03-04, AC-DSP-03-05, AC-DSP-03-06, AC-DSP-03-07, AC-DSP-03-08 | TC-DSP-03-01, TC-DSP-03-02, TC-DSP-03-03, TC-DSP-03-04, TC-DSP-03-05, TC-DSP-03-06, TC-DSP-03-07, TC-DSP-03-08 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-03.md) |
| FR-DSP-04 | Phân công trách nhiệm xử lý | MS-03 | AC-DSP-04-01, AC-DSP-04-02, AC-DSP-04-03, AC-DSP-04-04, AC-DSP-04-05, AC-DSP-04-06, AC-DSP-04-07, AC-DSP-04-08 | TC-DSP-04-01, TC-DSP-04-02, TC-DSP-04-03, TC-DSP-04-04, TC-DSP-04-05, TC-DSP-04-06, TC-DSP-04-07, TC-DSP-04-08 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-04.md) |
| FR-DSP-05 | Đặt hạn xử lý dự kiến | MS-03 | AC-DSP-05-01, AC-DSP-05-02, AC-DSP-05-03, AC-DSP-05-04, AC-DSP-05-05, AC-DSP-05-06, AC-DSP-05-07, AC-DSP-05-08 | TC-DSP-05-01, TC-DSP-05-02, TC-DSP-05-03, TC-DSP-05-04, TC-DSP-05-05, TC-DSP-05-06, TC-DSP-05-07, TC-DSP-05-08 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-05.md) |
| FR-DSP-06 | Bắt đầu xử lý yêu cầu | MS-03 | AC-DSP-06-01, AC-DSP-06-02, AC-DSP-06-03, AC-DSP-06-04, AC-DSP-06-05, AC-DSP-06-06 | TC-DSP-06-01, TC-DSP-06-02, TC-DSP-06-03, TC-DSP-06-04, TC-DSP-06-05, TC-DSP-06-06 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-06.md) |
| FR-DSP-07 | Yêu cầu sinh viên bổ sung thông tin | MS-03 | AC-DSP-07-01, AC-DSP-07-02, AC-DSP-07-03, AC-DSP-07-04, AC-DSP-07-05, AC-DSP-07-06, AC-DSP-07-07 | TC-DSP-07-01, TC-DSP-07-02, TC-DSP-07-03, TC-DSP-07-04, TC-DSP-07-05, TC-DSP-07-06, TC-DSP-07-07 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-07.md) |
| FR-DSP-08 | Ghi cập nhật tiến độ xử lý | MS-03 | AC-DSP-08-01, AC-DSP-08-02, AC-DSP-08-03, AC-DSP-08-04, AC-DSP-08-05, AC-DSP-08-06 | TC-DSP-08-01, TC-DSP-08-02, TC-DSP-08-03, TC-DSP-08-04, TC-DSP-08-05, TC-DSP-08-06 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-08.md) |
| FR-DSP-09 | Ghi kết quả giải quyết | MS-04 | AC-DSP-09-01, AC-DSP-09-02, AC-DSP-09-03, AC-DSP-09-04, AC-DSP-09-05, AC-DSP-09-06, AC-DSP-09-07 | TC-DSP-09-01, TC-DSP-09-02, TC-DSP-09-03, TC-DSP-09-04, TC-DSP-09-05, TC-DSP-09-06, TC-DSP-09-07 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-09.md) |
| FR-DSP-10 | Đóng yêu cầu đã giải quyết | MS-04 | AC-DSP-10-01, AC-DSP-10-02, AC-DSP-10-03, AC-DSP-10-04, AC-DSP-10-05, AC-DSP-10-06 | TC-DSP-10-01, TC-DSP-10-02, TC-DSP-10-03, TC-DSP-10-04, TC-DSP-10-05, TC-DSP-10-06 | [Đọc](../../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-10.md) |
| FR-RPT-01 | Tổng hợp trạng thái yêu cầu | MS-05 | AC-RPT-01-01, AC-RPT-01-02, AC-RPT-01-03, AC-RPT-01-04, AC-RPT-01-05, AC-RPT-01-06 | TC-RPT-01-01, TC-RPT-01-02, TC-RPT-01-03, TC-RPT-01-04, TC-RPT-01-05, TC-RPT-01-06 | [Đọc](../../docs/03-modules/03-management-reporting/requirements/FR-RPT-01.md) |
| FR-RPT-02 | Theo dõi yêu cầu quá hạn | MS-05 | AC-RPT-02-01, AC-RPT-02-02, AC-RPT-02-03, AC-RPT-02-04, AC-RPT-02-05, AC-RPT-02-06 | TC-RPT-02-01, TC-RPT-02-02, TC-RPT-02-03, TC-RPT-02-04, TC-RPT-02-05, TC-RPT-02-06 | [Đọc](../../docs/03-modules/03-management-reporting/requirements/FR-RPT-02.md) |
| FR-RPT-03 | Thống kê thời gian xử lý | MS-05 | AC-RPT-03-01, AC-RPT-03-02, AC-RPT-03-03, AC-RPT-03-04, AC-RPT-03-05, AC-RPT-03-06 | TC-RPT-03-01, TC-RPT-03-02, TC-RPT-03-03, TC-RPT-03-04, TC-RPT-03-05, TC-RPT-03-06 | [Đọc](../../docs/03-modules/03-management-reporting/requirements/FR-RPT-03.md) |
| FR-RPT-04 | Thống kê vấn đề phổ biến | MS-05 | AC-RPT-04-01, AC-RPT-04-02, AC-RPT-04-03, AC-RPT-04-04, AC-RPT-04-05, AC-RPT-04-06 | TC-RPT-04-01, TC-RPT-04-02, TC-RPT-04-03, TC-RPT-04-04, TC-RPT-04-05, TC-RPT-04-06 | [Đọc](../../docs/03-modules/03-management-reporting/requirements/FR-RPT-04.md) |
| FR-RPT-05 | Thống kê khối lượng theo phòng ban | MS-05 | AC-RPT-05-01, AC-RPT-05-02, AC-RPT-05-03, AC-RPT-05-04, AC-RPT-05-05, AC-RPT-05-06 | TC-RPT-05-01, TC-RPT-05-02, TC-RPT-05-03, TC-RPT-05-04, TC-RPT-05-05, TC-RPT-05-06 | [Đọc](../../docs/03-modules/03-management-reporting/requirements/FR-RPT-05.md) |
| FR-RPT-06 | Tổng hợp mức hài lòng | MS-05 | AC-RPT-06-01, AC-RPT-06-02, AC-RPT-06-03, AC-RPT-06-04, AC-RPT-06-05, AC-RPT-06-06, AC-RPT-06-07 | TC-RPT-06-01, TC-RPT-06-02, TC-RPT-06-03, TC-RPT-06-04, TC-RPT-06-05, TC-RPT-06-06, TC-RPT-06-07 | [Đọc](../../docs/03-modules/03-management-reporting/requirements/FR-RPT-06.md) |
| FR-IAM-01 | Đăng nhập | MS-01 | AC-IAM-01-01, AC-IAM-01-02, AC-IAM-01-03, AC-IAM-01-04, AC-IAM-01-05, AC-IAM-01-06 | TC-IAM-01-01, TC-IAM-01-02, TC-IAM-01-03, TC-IAM-01-04, TC-IAM-01-05, TC-IAM-01-06 | [Đọc](../../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-01.md) |
| FR-IAM-02 | Đăng xuất | MS-01 | AC-IAM-02-01, AC-IAM-02-02, AC-IAM-02-03, AC-IAM-02-04 | TC-IAM-02-01, TC-IAM-02-02, TC-IAM-02-03, TC-IAM-02-04 | [Đọc](../../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-02.md) |
| FR-IAM-03 | Kiểm soát quyền theo vai trò và hồ sơ | MS-01 | AC-IAM-03-01, AC-IAM-03-02, AC-IAM-03-03, AC-IAM-03-04, AC-IAM-03-05, AC-IAM-03-06 | TC-IAM-03-01, TC-IAM-03-02, TC-IAM-03-03, TC-IAM-03-04, TC-IAM-03-05, TC-IAM-03-06 | [Đọc](../../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-03.md) |

## Mã task theo FR

`.PM.01`: đặc tả/contract; `.BE.01`: mapping; `.BE.02`: triển khai; `.BE.03`: quyền/lỗi/self-check; `.FE.01`: màn hình; `.FE.02`: tích hợp API; `.QA.01`: fixture; `.QA.02`: thực thi TC; `.QA.03`: report/bug. Effort nằm ở 250 task công việc, 6 milestone có effort 0h.

## Kiểm tra hệ thống

| Kiểm tra | Task thực thi | Kết quả hiện có |
| --- | --- | --- |
| E2E-01 — Yêu cầu đến kết quả | QA-04 | Not Run |
| E2E-02 — Bổ sung thông tin | QA-04 | Not Run |
| E2E-03 — Phối hợp đổi trách nhiệm | QA-04 | Not Run |
| E2E-04 — Hạn và báo cáo | QA-04 | Not Run |
| E2E-05 — Đối chiếu 6 báo cáo | QA-04 | Not Run |
| E2E-06 — Quyền xuyên luồng | QA-04 | Not Run |
| E2E-07 — Không bỏ sót/gửi trùng | QA-04 | Not Run |
| E2E-08 — Chạy lại để bàn giao | QA-10 | Not Run |
| NFR-SEC-01 — Bảo vệ phiên, quyền và dữ liệu | QA-05 | Not Run |
| NFR-REL-01 — Nhất quán và gửi lặp | QA-05 | Not Run |
| NFR-USAB-01 — Dùng trên desktop và mobile | QA-05 | Not Run |
| NFR-PERF-01 — Đáp ứng đủ dùng cho demo | QA-05 | Not Run |
| NFR-OBS-01 — Truy vết lỗi và nghiệp vụ | QA-05 | Not Run |
| NFR-DEP-01 — Chạy lại và bàn giao prototype | QA-10 | Not Run |
| MET-01 — Có người chịu trách nhiệm | QA-06 | Not Run |
| MET-02 — Theo dõi trạng thái | QA-06 | Not Run |
| MET-03 — Giảm hỏi lại tiến độ | QA-06 | Not Run |
| MET-04 — Giảm bỏ sót/trùng | QA-06 | Not Run |
