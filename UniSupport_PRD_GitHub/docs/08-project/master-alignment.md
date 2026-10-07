# Hợp đồng liên kết với Team Charter / Master

Master lấy 24 FR từ requirements.json, không tự tạo tên/mã chức năng khác. Mỗi parent FR có công việc PM đặc tả/review, BE, FE và QA 4 TC. Việc dùng chung gồm chốt scope/thiết kế, setup/seed/tích hợp, soạn fixture/case, E2E/NFR/MET, retest, tài liệu và các mốc 15 tuần. Không tính việc dùng chung là module sản phẩm mới.

Phụ thuộc trong FR mô tả luồng nghiệp vụ; không biến toàn bộ đồ thị đó thành chuỗi develop theo thứ tự sử dụng. Ví dụ IAM-03 cần service xác thực, còn STU-04 cần câu hỏi của DSP-07 khi test, không phải chặn viết FE đến khi toàn bộ workflow xong. Master dùng gate PM → BE/FE → QA và các phụ thuộc giao diện/contract thực tế; mọi link phải tồn tại, không được bỏ link để né giới hạn import.

Team Charter mô tả cách nhóm dùng FR/task/TC để làm việc, review, quản lý thay đổi và bàn giao; không lặp đề xuất kinh doanh hay bịa người ký. Bug/retest phải truy về FR/TC. Parent effort bằng tổng con, parent không tính lại vào tổng 288h; 32h dự phòng giữ riêng chưa phân cho task hay coi đã dùng.
