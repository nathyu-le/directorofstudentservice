# 03-coordinate-responsibility

Điều phối đọc DSP-02 → DSP-04 chọn đơn vị/người mới và lý do → chuyển quyền/lịch sử cùng giao dịch. Bộ trách nhiệm nguồn còn hiệu lực trước commit, bộ đích có hiệu lực sau commit. Không có luồng phê duyệt nhiều cấp tự bịa.

Tiền điều kiện: seed có đúng vai trò và dữ liệu trong phạm vi; phiên còn hiệu lực. Sau mỗi bước đọc lại hồ sơ, kiểm tra trách nhiệm, trạng thái, lịch sử và quyền. Nếu một bước lỗi, không tuyên bố hoàn tất luồng; ghi case lỗi và tạo việc sửa/retest.
