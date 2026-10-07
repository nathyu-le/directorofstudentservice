# Ma trận truy vết yêu cầu

| FR | Module | Tên chức năng | BR | Workflow | AC | TS |
| --- | --- | --- | --- | --- | --- | --- |
| FR-IAM-01 | M01 | Đăng nhập | BR-01, BR-11 | Kiểm riêng | AC-IAM-01-01 đến AC-IAM-01-05 | TS-IAM-01-01 đến TS-IAM-01-05 |
| FR-IAM-02 | M01 | Đăng xuất | BR-01, BR-11 | Kiểm riêng | AC-IAM-02-01 đến AC-IAM-02-04 | TS-IAM-02-01 đến TS-IAM-02-04 |
| FR-IAM-03 | M01 | Kiểm soát quyền truy cập | BR-01, BR-02 | Kiểm riêng | AC-IAM-03-01 đến AC-IAM-03-06 | TS-IAM-03-01 đến TS-IAM-03-06 |
| FR-STU-01 | M02 | Xem danh sách yêu cầu của tôi | BR-01, BR-12 | WF-01 | AC-STU-01-01 đến AC-STU-01-05 | TS-STU-01-01 đến TS-STU-01-05 |
| FR-STU-02 | M02 | Sinh viên gửi yêu cầu hỗ trợ | BR-03, BR-08, BR-09, BR-10 | WF-01 | AC-STU-02-01 đến AC-STU-02-07 | TS-STU-02-01 đến TS-STU-02-07 |
| FR-STU-03 | M02 | Xem chi tiết và tiến độ yêu cầu của tôi | BR-01, BR-04, BR-07 | WF-01, WF-03, WF-06 | AC-STU-03-01 đến AC-STU-03-06 | TS-STU-03-01 đến TS-STU-03-06 |
| FR-TKT-01 | M03 | Xem hàng chờ và tìm kiếm Ticket | BR-01, BR-02, BR-12 | WF-02, WF-04 | AC-TKT-01-01 đến AC-TKT-01-05 | TS-TKT-01-01 đến TS-TKT-01-05 |
| FR-TKT-02 | M03 | Điều chỉnh loại vấn đề của Ticket | BR-05, BR-08, BR-09, BR-10 | Kiểm riêng | AC-TKT-02-01 đến AC-TKT-02-05 | TS-TKT-02-01 đến TS-TKT-02-05 |
| FR-TKT-03 | M03 | Xem chi tiết Ticket nội bộ | BR-01, BR-02, BR-04 | WF-02, WF-04, WF-05 | AC-TKT-03-01 đến AC-TKT-03-05 | TS-TKT-03-01 đến TS-TKT-03-05 |
| FR-TKT-04 | M03 | Xem lịch sử nghiệp vụ Ticket | BR-01, BR-08 | Kiểm riêng | AC-TKT-04-01 đến AC-TKT-04-05 | TS-TKT-04-01 đến TS-TKT-04-05 |
| FR-OPS-01 | M04 | Nhận Ticket từ hàng chờ | BR-02, BR-04, BR-09, BR-10 | WF-02 | AC-OPS-01-01 đến AC-OPS-01-05 | TS-OPS-01-01 đến TS-OPS-01-05 |
| FR-OPS-02 | M04 | Bắt đầu xử lý Ticket được phân công | BR-02, BR-04, BR-09, BR-10 | WF-02, WF-04 | AC-OPS-02-01 đến AC-OPS-02-05 | TS-OPS-02-01 đến TS-OPS-02-05 |
| FR-OPS-03 | M04 | Ghi cập nhật tiến độ xử lý | BR-02, BR-08, BR-09, BR-10 | WF-02 | AC-OPS-03-01 đến AC-OPS-03-05 | TS-OPS-03-01 đến TS-OPS-03-05 |
| FR-OPS-04 | M04 | Hoàn tất xử lý Ticket | BR-04, BR-07, BR-08, BR-09, BR-10 | WF-05, WF-06 | AC-OPS-04-01 đến AC-OPS-04-06 | TS-OPS-04-01 đến TS-OPS-04-06 |
| FR-OPS-05 | M04 | Từ chối xử lý Ticket | BR-04, BR-07, BR-08, BR-09, BR-10 | WF-05, WF-06 | AC-OPS-05-01 đến AC-OPS-05-06 | TS-OPS-05-01 đến TS-OPS-05-06 |
| FR-ASG-01 | M05 | Phân công người phụ trách Ticket | BR-02, BR-04, BR-09, BR-10 | WF-02, WF-04 | AC-ASG-01-01 đến AC-ASG-01-06 | TS-ASG-01-01 đến TS-ASG-01-06 |
| FR-ASG-02 | M05 | Đổi người phụ trách Ticket | BR-02, BR-05, BR-09, BR-10 | Kiểm riêng | AC-ASG-02-01 đến AC-ASG-02-06 | TS-ASG-02-01 đến TS-ASG-02-06 |
| FR-ASG-03 | M05 | Chuyển Ticket sang phòng ban khác | BR-01, BR-05, BR-06, BR-09, BR-10 | WF-04 | AC-ASG-03-01 đến AC-ASG-03-07 | TS-ASG-03-01 đến TS-ASG-03-07 |
| FR-COM-01 | M06 | Yêu cầu sinh viên bổ sung thông tin | BR-04, BR-07, BR-09, BR-10 | WF-03 | AC-COM-01-01 đến AC-COM-01-05 | TS-COM-01-01 đến TS-COM-01-05 |
| FR-COM-02 | M06 | Sinh viên trả lời yêu cầu bổ sung | BR-04, BR-07, BR-09, BR-10 | WF-03 | AC-COM-02-01 đến AC-COM-02-06 | TS-COM-02-01 đến TS-COM-02-06 |
| FR-COM-03 | M06 | Xem hội thoại bổ sung của Ticket | BR-01, BR-07, BR-12 | Kiểm riêng | AC-COM-03-01 đến AC-COM-03-05 | TS-COM-03-01 đến TS-COM-03-05 |
| FR-NOT-01 | M07 | Tạo thông báo trong ứng dụng theo sự kiện | BR-08, BR-13 | WF-01, WF-02, WF-03, WF-04, WF-05, WF-06 | AC-NOT-01-01 đến AC-NOT-01-06 | TS-NOT-01-01 đến TS-NOT-01-06 |
| FR-NOT-02 | M07 | Xem danh sách thông báo của tôi | BR-01, BR-13 | Kiểm riêng | AC-NOT-02-01 đến AC-NOT-02-05 | TS-NOT-02-01 đến TS-NOT-02-05 |
| FR-NOT-03 | M07 | Đánh dấu một thông báo đã đọc | BR-01, BR-13 | Kiểm riêng | AC-NOT-03-01 đến AC-NOT-03-05 | TS-NOT-03-01 đến TS-NOT-03-05 |
| FR-SLA-01 | M08 | Tính hạn xử lý khi tạo Ticket | BR-03, BR-14 | WF-01 | AC-SLA-01-01 đến AC-SLA-01-05 | TS-SLA-01-01 đến TS-SLA-01-05 |
| FR-SLA-02 | M08 | Điều chỉnh hạn xử lý Ticket | BR-05, BR-09, BR-10, BR-14 | Kiểm riêng | AC-SLA-02-01 đến AC-SLA-02-06 | TS-SLA-02-01 đến TS-SLA-02-06 |
| FR-SLA-03 | M08 | Xác định và hiển thị Ticket quá hạn | BR-04, BR-13, BR-14 | Kiểm riêng | AC-SLA-03-01 đến AC-SLA-03-06 | TS-SLA-03-01 đến TS-SLA-03-06 |
| FR-FDB-01 | M09 | Sinh viên xác nhận đóng Ticket | BR-04, BR-07, BR-09, BR-10 | WF-06 | AC-FDB-01-01 đến AC-FDB-01-06 | TS-FDB-01-01 đến TS-FDB-01-06 |
| FR-FDB-02 | M09 | Sinh viên gửi đánh giá mức hài lòng | BR-07, BR-09, BR-10 | WF-06 | AC-FDB-02-01 đến AC-FDB-02-06 | TS-FDB-02-01 đến TS-FDB-02-06 |
| FR-RPT-01 | M10 | Thống kê Ticket theo trạng thái | BR-01, BR-12, BR-15 | Kiểm riêng | AC-RPT-01-01 đến AC-RPT-01-05 | TS-RPT-01-01 đến TS-RPT-01-05 |
| FR-RPT-02 | M10 | Xem danh sách Ticket quá hạn đang mở | BR-01, BR-12, BR-15 | Kiểm riêng | AC-RPT-02-01 đến AC-RPT-02-05 | TS-RPT-02-01 đến TS-RPT-02-05 |
| FR-RPT-03 | M10 | Tính thời gian giải quyết thành công trung bình | BR-01, BR-12, BR-15 | Kiểm riêng | AC-RPT-03-01 đến AC-RPT-03-06 | TS-RPT-03-01 đến TS-RPT-03-06 |
| FR-RPT-04 | M10 | Thống kê Ticket theo loại vấn đề | BR-01, BR-12, BR-15 | Kiểm riêng | AC-RPT-04-01 đến AC-RPT-04-05 | TS-RPT-04-01 đến TS-RPT-04-05 |
| FR-RPT-05 | M10 | Thống kê tải công việc theo nhân viên | BR-01, BR-12, BR-15 | Kiểm riêng | AC-RPT-05-01 đến AC-RPT-05-06 | TS-RPT-05-01 đến TS-RPT-05-06 |
| FR-RPT-06 | M10 | Thống kê mức hài lòng của sinh viên | BR-01, BR-12, BR-15 | Kiểm riêng | AC-RPT-06-01 đến AC-RPT-06-07 | TS-RPT-06-01 đến TS-RPT-06-07 |

## Truy vết yêu cầu phi chức năng

| NFR | Mã kiểm tra | Liên hệ chức năng |
| --- | --- | --- |
| NFR-PER-01 | NT-PER-01 | Mọi FR liên quan; phương pháp tại phần 5 Hiệu năng đọc |
| NFR-PER-02 | NT-PER-02 | Mọi FR liên quan; phương pháp tại phần 5 Hiệu năng ghi và báo cáo |
| NFR-SEC-01 | NT-SEC-01 | Mọi FR liên quan; phương pháp tại phần 5 Xác thực và phiên |
| NFR-SEC-02 | NT-SEC-02 | Mọi FR liên quan; phương pháp tại phần 5 Quyền và dữ liệu nhập |
| NFR-SEC-03 | NT-SEC-03 | Mọi FR liên quan; phương pháp tại phần 5 Bảo vệ bí mật |
| NFR-REL-01 | NT-REL-01 | Mọi FR liên quan; phương pháp tại phần 5 Tính toàn vẹn |
| NFR-REL-02 | NT-REL-02 | Mọi FR liên quan; phương pháp tại phần 5 Phục hồi thông báo |
| NFR-REL-03 | NT-REL-03 | Mọi FR liên quan; phương pháp tại phần 5 Sao lưu và phục hồi prototype |
| NFR-USE-01 | NT-USE-01 | Mọi FR liên quan; phương pháp tại phần 5 Khả năng hoàn thành tác vụ |
| NFR-USE-02 | NT-USE-02 | Mọi FR liên quan; phương pháp tại phần 5 Giao diện trên kích thước màn hình |
| NFR-OBS-01 | NT-OBS-01 | Mọi FR liên quan; phương pháp tại phần 5 Log để điều tra |
| NFR-OBS-02 | NT-OBS-02 | Mọi FR liên quan; phương pháp tại phần 5 Quan sát tác vụ nền |
| NFR-DEP-01 | NT-DEP-01 | Mọi FR liên quan; phương pháp tại phần 5 Khả năng cài và bàn giao |
| NFR-DEP-02 | NT-DEP-02 | Mọi FR liên quan; phương pháp tại phần 5 Môi trường kiểm thử xác định |

## Đối chiếu mã của PRD trước

| Mã cũ | Mã FR mới |
| --- | --- |
| A01 | FR-IAM-01 |
| A02 | FR-IAM-02 |
| A03 | FR-IAM-03 |
| S01 | FR-STU-02 |
| S02 | FR-STU-01 |
| S03 | FR-STU-03 |
| S04 | FR-COM-02 |
| S05 | FR-FDB-02 |
| D01 | FR-TKT-01 |
| D02 | FR-TKT-02 |
| D03 | FR-ASG-01 |
| D04 | FR-ASG-02 |
| D05 | FR-SLA-02 |
| D06 | FR-TKT-01 |
| D07 | FR-OPS-02 |
| D08 | FR-OPS-03 |
| D09 | FR-COM-01 |
| D10 | FR-OPS-04 |
| D11 | FR-TKT-04 |
| R01 | FR-RPT-01 |
| R02 | FR-RPT-02 |
| R03 | FR-RPT-03 |
| R04 | FR-RPT-04 |
| R05 | FR-RPT-05 |
| R06 | FR-RPT-06 |


D01 hàng chờ và D06 công việc được giao là hai góc nhìn của cùng FR-TKT-01, không tạo hai chức năng cùng hành vi. S04 chuyển sang FR-COM-02, S05 chuyển sang FR-FDB-02 và chỉ cho phép sau CLOSED. D10 chuyển sang FR-OPS-04; từ chối có mã riêng FR-OPS-05. Các mã mới bổ sung không có đối ứng ở bản cũ, dùng đúng phạm vi đã mô tả trong phần sản phẩm.

requirements.csv và requirements.json chứa đúng danh mục 35 FR cùng tên, module, phụ thuộc triển khai và AC/TS để chuẩn bị Master Plan. dependencies là tiền đề triển khai, không thay các preconditions nghiệp vụ; một FR có thể tham gia nhiều nhánh workflow. Mối nối chạy thực tế giữa tạo Ticket, SLA và notification vẫn phải kiểm tích hợp dù tiền đề triển khai không tạo vòng phụ thuộc.

[Về danh mục PRD](../README.md)
