# Kịch bản Kiểm thử chính (Test Scenarios)

| ID | Kịch bản Kiểm thử (Test Case) | Dữ liệu đầu vào (Input) | Kết quả mong đợi (Expected Output) | Trạng thái |
|---|---|---|---|---|
| TC-01 | Sinh viên tạo Ticket thành công | Form đầy đủ thông tin hợp lệ | Ticket xuất hiện ở DB với mã UID, hiển thị trên DS của SV | *Pending* |
| TC-02 | Chống spam gửi yêu cầu | Bấm nút Submit liên tục 5 lần | DB chỉ ghi nhận 1 bản ghi duy nhất | *Pending* |
| TC-03 | Quyền truy cập URL chéo | SV_A gõ URL chi tiết Ticket của SV_B | Hiển thị màn hình Lỗi 403 (Access Denied) | *Pending* |
| TC-04 | Đổi người phụ trách | QL đổi Ticket từ NV_A sang NV_B | Lịch sử ghi nhận; NV_B thấy Ticket, NV_A mất quyền sửa | *Pending* |
| TC-05 | Bắt buộc nhập lý do bổ sung | NV bấm "Chờ bổ sung" nhưng bỏ trống lý do | Nút Submit bị vô hiệu, báo lỗi đỏ tại ô Textbox | *Pending* |