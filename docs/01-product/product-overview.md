# Tổng quan Sản phẩm (Product Overview)

## 1. Tầm nhìn (Vision)
UniSupport là hệ thống quản lý hỗ trợ sinh viên tập trung của Aurora University, giúp tự động hóa quy trình tiếp nhận, phân công và theo dõi yêu cầu. Hệ thống xóa bỏ tình trạng thất lạc hồ sơ và minh bạch hóa tiến độ xử lý cho sinh viên.

## 2. Mục tiêu (Goals) & Đo lường (KPIs)
| Mã | Mục tiêu | Phương pháp đo lường (Tuần 14) |
|---|---|---|
| **O1** | 90% yêu cầu có người phụ trách rõ ràng | (Số Ticket có Assignee / Tổng Ticket trong bộ test) * 100% >= 90%. |
| **O2** | 90% yêu cầu có trạng thái minh bạch | Giao diện SV hiển thị chính xác trạng thái thực tế từ DB. |
| **O3** | Giảm 30% lượt hỏi tiến độ qua kênh khác | (Lượt hỏi trước - Lượt hỏi sau) / Lượt hỏi trước * 100%. |
| **O4** | Không bỏ sót, không xử lý trùng lặp | Ghi nhận 0 ca bỏ sót hoặc 2 người cùng xử lý 1 hồ sơ trong bộ test. |

## 3. Vai trò Người dùng (Actors)
1. **Sinh viên (SV):** Gửi, bổ sung thông tin, theo dõi tiến độ, đánh giá mức độ hài lòng.
2. **Nhân viên (NV):** Tiếp nhận ticket được giao, yêu cầu bổ sung, cập nhật kết quả xử lý.
3. **Quản lý (QL):** Phân loại, điều phối ticket, xem báo cáo thống kê và kiểm soát tải công việc.