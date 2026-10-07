# Triển khai và bàn giao

## NFR-DEP-01 Khả năng cài và bàn giao

**Yêu cầu:** Gói source PHP/MySQL có hướng dẫn cài, cấu hình qua environment, schema/migration, seed, cách chạy job và thao tác sao lưu. Không cần tài khoản production thật.

**Cách kiểm chứng:** Người kiểm tra dùng môi trường trống, làm theo README, đăng nhập ba vai trò và chạy smoke test tạo, tiếp nhận, kết thúc, đóng, đánh giá.

## NFR-DEP-02 Môi trường kiểm thử xác định

**Yêu cầu:** BE ghi phiên bản PHP, MySQL, trình duyệt, cấu hình máy và dependency thực dùng tại nghiệm thu; môi trường phát triển, test và demo dùng dữ liệu riêng.

**Cách kiểm chứng:** Đối chiếu tài liệu môi trường với hệ thống đang chạy; không ghi phiên bản giả định như đã được kiểm chứng.

[Về danh mục PRD](../README.md)
