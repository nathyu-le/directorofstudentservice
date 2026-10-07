# Vòng đời và Trạng thái Yêu cầu (Ticket Lifecycle)

## 1. Ma trận Trạng thái (State Machine)
Mọi Ticket trong UniSupport phải tuân thủ nghiêm ngặt luồng trạng thái sau:

| Trạng thái hiện tại | Hành động (Trigger) | Người thực hiện | Trạng thái tiếp theo | Ràng buộc nghiệp vụ |
|---|---|---|---|---|
| *Mới tạo* | Gửi yêu cầu | Sinh viên | **Đã tiếp nhận** | Dữ liệu form hợp lệ. |
| Đã tiếp nhận | Phân công/Nhận việc | Quản lý / NV | **Đang xử lý** | Ticket được gắn cho 1 NV cụ thể. |
| Đang xử lý | Yêu cầu bổ sung | Nhân viên | **Chờ bổ sung** | Bắt buộc nhập lý do cần bổ sung. |
| Chờ bổ sung | Cập nhật thông tin | Sinh viên | **Đang xử lý** | Sinh viên submit form bổ sung. |
| Đang xử lý | Hoàn tất giải quyết | Nhân viên | **Đã giải quyết** | Phải nhập kết quả xử lý chi tiết. |

## 2. Lịch sử thay đổi (Audit Log)
Mọi thao tác chuyển trạng thái phải được hệ thống tự động ghi log bao gồm: `Thời điểm (Timestamp)`, `Người thực hiện (User ID)`, `Trạng thái cũ`, `Trạng thái mới`, `Ghi chú (nếu có)`.

## 3. Sơ đồ Chuyển trạng thái (State Machine Diagram)
```mermaid
stateDiagram-v2
    [*] --> DaTiepNhan : SV tạo Ticket hợp lệ
    DaTiepNhan --> DangXuLy : NV Tiếp nhận
    DangXuLy --> ChoBoSung : NV yêu cầu thêm thông tin
    ChoBoSung --> DangXuLy : SV cập nhật thông tin
    DangXuLy --> DaGiaiQuyet : NV hoàn tất xử lý
    DaGiaiQuyet --> Dong : SV đánh giá / Auto-close sau 7 ngày
    Dong --> [*]