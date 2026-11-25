# Yêu cầu phi chức năng của PRD

Ngưỡng prototype theo thiết kế đề xuất. Phạm vi FR và điểm kiểm sớm dưới đây bổ sung vào đầu ra QA hiện có. MS-06 kiểm lại trên bản tích hợp. QA thực hiện các lượt theo phạm vi trên build tích hợp khả dụng, dùng chung fixture và phép đo với FR khi đủ bằng chứng. Task ghi ở cột cuối tổng hợp và kiểm bằng chứng trước khi đóng mốc. Phần kiểm tra bổ sung trong dự toán cũ phải được rà theo OQ-08/R-08.

| NFR | Điều kiện đo/đạt | Phạm vi | Cổng và công việc kiểm |
| --- | --- | --- | --- |
| NFR-SEC-01 | Mọi API kiểm tra phiên/vai trò/đối tượng; mật khẩu băm; truy vấn tham số hóa; output encode; chống CSRF cho thao tác ghi; không ghi mật khẩu/nội dung nhạy cảm vào log. | Toàn bộ 24 FR, theo tính chất của từng điểm truy cập và giao diện | MS-01: FR-IAM-03.QA.03; MS-02: QA-03; MS-03: FR-STU-04.QA.03; MS-04: FR-DSP-10.QA.03; MS-05: FR-RPT-06.QA.03; MS-06: QA-05 |
| NFR-REL-01 | Nghiệp vụ/lịch sử cùng transaction; kiểm tra phiên bản; gửi lặp không tạo hai yêu cầu/kết quả. | FR-STU-01, FR-STU-04, FR-STU-05, FR-DSP-03, FR-DSP-04, FR-DSP-05, FR-DSP-06, FR-DSP-07, FR-DSP-08, FR-DSP-09, FR-DSP-10 | MS-02: QA-03; MS-03: FR-STU-04.QA.03; MS-04: FR-DSP-10.QA.03; MS-06: QA-05 |
| NFR-USAB-01 | Luồng chính thao tác được ở viewport 1366 × 768 và 390 × 844; nhãn rõ, lỗi tại trường, điều hướng bàn phím, không mất dữ liệu vì lỗi kiểm tra. Các viewport là mục tiêu thử prototype đề xuất. | FR-STU-01, FR-STU-02, FR-STU-03, FR-STU-04, FR-STU-05, FR-DSP-07 | MS-02: QA-03; MS-03: FR-STU-04.QA.03; MS-04: FR-DSP-10.QA.03; MS-06: QA-05 |
| NFR-PERF-01 | Mục tiêu thử đề xuất: 95% trong 20 lần đo trang danh sách/chi tiết/báo cáo ≤3 giây với 1.000 yêu cầu mẫu trên máy demo ghi rõ cấu hình. Chưa là SLA production. | FR-STU-02, FR-STU-03, FR-DSP-01, FR-DSP-02, FR-RPT-01, FR-RPT-02, FR-RPT-03, FR-RPT-04, FR-RPT-05, FR-RPT-06 | MS-02: QA-03; MS-05: FR-RPT-06.QA.03; MS-06: QA-05 |
| NFR-OBS-01 | Có request/correlation ID cho lỗi; lịch sử nghiệp vụ có tác nhân/thời gian/hành động, không lộ mật khẩu hay token. | Toàn bộ 24 FR, theo tính chất của từng điểm truy cập và giao diện | MS-01: FR-IAM-03.QA.03; MS-02: QA-03; MS-03: FR-STU-04.QA.03; MS-04: FR-DSP-10.QA.03; MS-05: FR-RPT-06.QA.03; MS-06: QA-05 |
| NFR-DEP-01 | README nêu PHP/MySQL phiên bản được nhóm chốt, cấu hình .env.example không secret, schema/seed, cách khởi chạy và kiểm thử; nhóm khác chạy được trên môi trường ghi rõ. | Toàn bộ 24 FR, theo tính chất của từng điểm truy cập và giao diện | MS-06: QA-10 |

## Hành vi lỗi dùng chung

- NFR-SEC-01: đối với tuyến cần đăng nhập, 401 yêu cầu xác thực lại. Máy khách bỏ yêu cầu ghi đang chờ và đợi thao tác mới của người dùng. Kiểm 403 vì vai trò/CSRF ở các tuyến áp dụng, 404 ngoài quyền với hồ sơ, 422 với ID sai định dạng hoặc phiên bản đầu vào không hợp lệ. Kiểm bằng request có cùng payload và thay đúng một điều kiện xác thực mỗi lần. Lưu status, các trường phản hồi cho phép, ảnh chụp DB trước/sau.
- NFR-OBS-01: mô phỏng lỗi xử lý hoặc ngắt kết nối có kiểm soát ở từng loại tuyến. Đọc lại dữ liệu khi trạng thái giao dịch chưa biết. Giao diện không hiển thị thành công trước khi có xác nhận và không tự gửi lại thao tác ghi. STU-01 giữ khóa gửi riêng của nó. Với 422 trên biểu mẫu ghi đang mở, lỗi gắn đúng trường và giữ dữ liệu đã nhập. Với lỗi 500, mã đối chiếu khớp log đã loại thông tin nhạy cảm.
- NFR-REL-01: với tuyến ghi, chụp trạng thái hồi quy trước và sau lỗi trước commit, thử gửi lại và ghi đồng thời theo hành vi từng FR. Xem [cách giả lập](../04-acceptance/fixtures.md).

[Danh sách chạy và kết quả](../04-acceptance/system-tests.md).
