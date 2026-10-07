# Kịch bản xuyên module

| Mã | Kịch bản | FR | Bước | Expected |
| --- | --- | --- | --- | --- |
| E2E-01 | Yêu cầu đến kết quả | STU-01, DSP-01/03/04/06/08/09/10, STU-03/05 | Tạo → phân loại → giao → bắt đầu → tiến độ công khai → giải quyết → phản hồi → đóng. | Một request_id; đúng người/trạng thái/lịch sử; phản hồi tính đúng một lần. |
| E2E-02 | Bổ sung thông tin | DSP-07, STU-03/04, DSP-09 | Yêu cầu câu hỏi khi đang xử lý; SV đọc/trả lời; NV giải quyết. | Chờ bổ sung → Đang xử lý; giữ hồ sơ và trách nhiệm; không câu hỏi mở khi giải quyết. |
| E2E-03 | Phối hợp đổi trách nhiệm | DSP-04, IAM-03, DSP-08 | Giao PB-A/NV-A rồi chuyển PB-B/NV-B có lý do; thử NV-A và NV-B. | Chỉ người hiện hành sửa; lịch sử giữ bộ cũ/mới; không có khoảng dữ liệu hai người. |
| E2E-04 | Hạn và báo cáo | DSP-05, RPT-02, DSP-09 | Đặt hạn, đo trước/đúng/sau hạn, giải quyết rồi đo lại. | Đúng công thức quá hạn; yêu cầu kết thúc rời danh sách quá hạn. |
| E2E-05 | Đối chiếu 6 báo cáo | RPT-01..06 | Seed có kết quả đã biết; chạy từng báo cáo và đối chiếu query nguồn. | Mọi tổng, trung bình, mẫu, kỳ và phạm vi khớp định nghĩa; dữ liệu rỗng hiển thị đúng. |
| E2E-06 | Quyền xuyên luồng | IAM-01/02/03, STU/DSP/RPT | Đăng nhập từng vai trò; thử hành động hợp lệ và trái quyền; đăng xuất rồi gọi phiên cũ. | Không lộ hồ sơ/số liệu/ghi chú; phiên cũ không đọc/ghi. |
| E2E-07 | Không bỏ sót/gửi trùng | STU-01, DSP-01 | Gửi n yêu cầu với n khóa khác nhau, gửi lặp một khóa, đối chiếu hàng chờ và DB. | Có n hồ sơ duy nhất, mỗi mã ở hàng chờ trong phạm vi; không phát sinh n+1 do lặp. |
| E2E-08 | Chạy lại để bàn giao | NFR-DEP-01 và E2E-01 | Nhóm khác/một người không trực tiếp viết cài mới theo README và chạy E2E-01. | Chạy được bằng nguồn/seed/tài liệu; lỗi và bước thiếu ghi vào handoff, không giả ký nghiệm thu. |

Mỗi lần chạy: ghi precondition/seed/build/actor/thời gian, actual result, Result (Not Run/Pass/Fail/Blocked), evidence và bug ID. Thực hiện trên môi trường sạch có cấu hình ở README của prototype.
