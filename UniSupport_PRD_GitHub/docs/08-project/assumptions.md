# Giả định và cấu hình cần review

Phạm vi 4 module/mục tiêu/15 tuần/công nghệ lấy từ proposal. Các lựa chọn dưới đây là đề xuất cho prototype, không chép từ ảnh hay giả khách hàng đã duyệt.

| Mã | Đề xuất cấu hình |
| --- | --- |
| CFG-01 | Tiêu đề tối đa 150 ký tự |
| CFG-02 | Mô tả/câu hỏi/cập nhật/kết quả/nhận xét tối đa 3.000 ký tự |
| CFG-03 | Tìm kiếm tối đa 150 ký tự |
| CFG-04 | 20 dòng/trang; page là số nguyên ≥1 |
| CFG-05 | Điểm hài lòng nguyên 1–5 với nhãn từ Rất không hài lòng đến Rất hài lòng |
| CFG-06 | Hiển thị và diễn giải ngày theo Asia/Ho_Chi_Minh; lưu thời điểm nhất quán |
| CFG-07 | Email tối đa 254 ký tự |
| CFG-08 | Phiên hết hạn sau 30 phút không hoạt động |

- AS-01: vai trò điều phối tách với quản lý báo cáo; account và scope seed sẵn.
- AS-02: 5 trạng thái, không reopen; điều phối đóng sau khi có kết quả, phản hồi không tự đóng/mở lại.
- AS-03: phối hợp đổi đơn vị/người là một lần phân công có lý do, chưa có quy trình phê duyệt nhiều cấp.
- AS-04: hạn đặt thủ công để thử quá hạn, chưa có SLA số ngày của trường.
- AS-05: phản hồi có điểm để tính hài lòng; một bản hiện hành mỗi yêu cầu.
- AS-06: mốc ngày Master dự kiến 12/10/2026–22/01/2027 cho 15 tuần làm việc; chưa là ngày khởi động được xác nhận. Lịch Thứ Hai–Thứ Sáu; ngày nghỉ cần rà trước khi chốt.
- AS-07: estimate 288 giờ việc chính + 32 dự phòng là baseline nhóm đề xuất đã dùng để lập nguồn lực, không phải con số effort có trong proposal hay số giờ đã thực hiện.

Nếu review đổi giả định, cập nhật FR/AC/TC/Master liên quan; không tự thêm module. Những chi tiết nằm ngoài phạm vi chưa được đưa thành công việc triển khai.
