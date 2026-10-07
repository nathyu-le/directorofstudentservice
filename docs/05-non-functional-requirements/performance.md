# Hiệu năng

## NFR-PER-01 Hiệu năng đọc

**Yêu cầu:** Trong môi trường nghiệm thu cố định, 10000 Ticket, 30 phiên đồng thời, p95 thời gian từ gửi đến nhận response của danh sách và chi tiết không quá 2 giây.

**Cách kiểm chứng:** QA ghi cấu hình máy, phiên bản ứng dụng và database; chạy 5 phút làm nóng rồi 10 phút đo, mỗi phiên một request mỗi 2 giây. Không tính tải ảnh tĩnh. Ghi p50/p95, tỷ lệ lỗi; lỗi nghiệp vụ chủ động không tính là lỗi máy chủ.

## NFR-PER-02 Hiệu năng ghi và báo cáo

**Yêu cầu:** Cùng môi trường, p95 thao tác ghi không quá 3 giây, sáu báo cáo không quá 5 giây; tỷ lệ lỗi máy chủ dưới 1% trong phép đo.

**Cách kiểm chứng:** Đo endpoint riêng theo tải đồng thời như NFR-PER-01; dữ liệu ghi mới dùng operation_id khác nhau. Retry và xung đột được kiểm như nghiệp vụ, không tăng giả số thành công.

[Về danh mục PRD](../README.md)
