# Mô hình dữ liệu đề xuất

| Bảng | Trường chính | Ràng buộc |
| --- | --- | --- |
| users | id, email(unique), password_hash, active | Tài khoản seed; không lưu password thô |
| user_roles / user_scopes | user_id, role, department_id | Quyền server; không nâng quyền client |
| departments / categories | id, name, active | Danh mục seed, không CRUD ngoài phạm vi |
| support_requests | id, reference(unique), student_id, category_id, department_id, assignee_id, title, description, status, due_at, created_at, started_at, resolved_at, closed_at, version | Ràng buộc quan hệ/trạng thái; index chủ, trách nhiệm, trạng thái, hạn |
| submission_keys | user_id, submission_key, request_id | Unique(user_id, submission_key); chống gửi lặp |
| request_events | id, request_id, actor_id, action, visibility, before/after, created_at | Không xóa lịch sử; lọc visibility |
| information_questions | id, request_id, question, answer, asked_by, answered_by, asked_at, answered_at | Tối đa một câu hỏi mở; gắn chủ đúng |
| resolutions | request_id, content, resolved_by, resolved_at | Một kết quả hiện hành theo baseline không reopen |
| feedback | request_id(unique), student_id, score, comment, updated_at | Một phản hồi hiện hành; score nguyên 1–5 |

Khóa ngoại không cho tạo bản bổ sung/feedback mồ côi. Mọi thời điểm lưu nhất quán, chuyển sang CFG-06 khi hiển thị. Kiểm tra department–assignee và record_version ở transaction; không giao hai người do hai lần cập nhật đồng thời. Tên bảng là gợi ý triển khai, không là bảng đã tồn tại trong ứng dụng.
