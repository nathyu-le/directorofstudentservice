# Luồng quy trình chính: Tạo và Xử lý Ticket

1. **Khởi tạo:** Sinh viên đăng nhập -> Chọn "Gửi yêu cầu" -> Điền Form -> Submit.
2. **Tiếp nhận:** Hệ thống ghi nhận vào DB (Trạng thái: Đã tiếp nhận) -> Thông báo đến Quản lý phòng ban tương ứng.
3. **Phân công:** Quản lý xem danh sách -> Gán Ticket cho Nhân viên A.
4. **Xử lý:** Nhân viên A xem chi tiết Ticket -> Bấm "Tiếp nhận xử lý" (Trạng thái: Đang xử lý).
    - *Ngoại lệ:* Nếu thiếu thông tin -> NV chọn "Yêu cầu bổ sung" -> Chờ SV cập nhật.
5. **Giải quyết:** NV A xử lý xong ngoài thực tế -> Nhập kết quả vào hệ thống -> Bấm "Hoàn tất" (Trạng thái: Đã giải quyết).
6. **Đánh giá:** SV nhận thông báo hoàn tất -> Xem kết quả -> Chấm điểm hài lòng. Ticket đóng vòng đời.

## Sơ đồ Luồng nghiệp vụ (Business Process Flow)

```mermaid
flowchart TD
    %% Định nghĩa các Actor
    SV([Sinh viên])
    HeThong[[Hệ thống UniSupport]]
    QL([Quản lý])
    NV([Nhân viên])

    %% Luồng chạy
    SV -->|Điền Form & Gửi| HeThong
    HeThong -->|Validation| Check{Dữ liệu hợp lệ?}
    Check -->|Không| B[Báo lỗi Form] --> SV
    Check -->|Có| C[Tạo Ticket: Đã tiếp nhận]
    
    C -->|Thông báo| QL
    QL -->|Assign/Gán việc| D[Phân công cho Nhân viên]
    D --> NV
    
    NV -->|Bấm Tiếp nhận| E[Trạng thái: Đang xử lý]
    E --> F{Đủ thông tin chưa?}
    
    F -->|Thiếu| G[Trạng thái: Chờ bổ sung] -->|Thông báo| SV
    SV -->|Cập nhật form| E
    
    F -->|Đủ| H[Xử lý nghiệp vụ thực tế]
    H --> I[Nhập kết quả & Hoàn tất]
    I --> J[Trạng thái: Đã giải quyết]
    
    J -->|Gửi Email/In-app| SV
    SV -->|Đánh giá hài lòng| K[Trạng thái: Đóng]