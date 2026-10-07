# Mô hình yêu cầu

| Trường | Quy tắc |
| --- | --- |
| id / reference | ID nội bộ và mã hiển thị duy nhất |
| student_id | Lấy từ phiên khi tạo, không do client chọn |
| category_id | Loại hiện hành; có giá trị Chưa xác định |
| title / description | Nội dung do sinh viên cung cấp; kiểm tra CFG |
| department_id / assignee_id | Ban đầu có thể trống; khi gán phải cùng hợp lệ trong một giao dịch |
| status | Một trong 5 trạng thái đề xuất |
| created_at / started_at / resolved_at / closed_at | Thời điểm thật của sự kiện; không gán trước |
| due_at | Có thể trống; không trước created_at |
| record_version | Tăng khi sửa dữ liệu; phát hiện cập nhật cũ |

Quan hệ: yêu cầu 1–n lịch sử, 1–n câu hỏi/trả lời bổ sung, 1–n cập nhật; 0–1 kết quả hiện hành; 0–1 phản hồi hiện hành. Không xóa hồ sơ nghiệp vụ trong prototype. Định nghĩa bảng ở data-model.md.
