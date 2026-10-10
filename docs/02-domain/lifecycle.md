# Vòng đời và bất biến

5trạng thái dưới đây là thiết kế đề xuất cần review OQ-02.

| Mã | Nhãn | Hành động vào trạng thái | Điều kiện |
| --- | --- | --- | --- |
| Received | Đã tiếp nhận | STU-01 | Tạo request+history cùng transaction |
| Processing | Đang xử lý | DSP-06 hoặc STU-04 | Người được giao bắt đầu hoặc chủ SV trả lời question mở |
| WaitingInfo | Chờ bổ sung | DSP-07 | Processing, câu hỏi hợp lệ, không question mở khác |
| Resolved | Đã giải quyết | DSP-09 | Processing, có result, không question mở |
| Closed | Đã đóng | DSP-10 | Dispatch trongscope, Resolved+có result |

Chỉ cho các cạnh: Received→Processing→WaitingInfo→Processing→Resolved→Closed. Không tự mở lại, hủy, đóng theo thời gian. Phân loại/phân công/hạn/tiến độ/phản hồi giữ trạng thái; STU-04 không reset started_at. Còn mở=Received/Processing/WaitingInfo. Kết thúc=Resolved/Closed.

Mọi POST sửa request có record_version>=1 trừcreate/login/logout. Với feedback cũng kiểm request.version. Transaction khóa request rồi đọc lại object scope, trạng thái/version, ghi dữ liệu con+history, tăng version và commit. Phiên bản cũ trả 409 STALE_VERSION, không sửa phần còn lại. Server đồng hồ UTC, UI Asia/Ho_Chi_Minh. Lịch sử append-only; không xóa hồ sơ trong baseline.

Các thay đổi no_change ở DSP-03/04/05 kiểm quyền/state/version trước, không tăng version hoặc history. Các chuyển trạng thái gửi lặp trả 409. STU-01 có replay200 theoidempotency; logout204 lặp an toàn. Không thay tất cả hành vi bằng một quy tắc retry mơ hồ.
