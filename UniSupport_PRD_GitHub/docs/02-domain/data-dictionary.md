# Data dictionary và seed

Các bảng/trường là thiết kế prototype đề xuất. ID BIGINT UNSIGNED; FK ON DELETE RESTRICT; utf8mb4; mọi datetime lưu UTC, API ISO8601Z. Nội dung text thuần, escape khi HTML, SQL prepared statements.

| Bảng | Trường và kiểu | Bắt buộc, nguồn, ràng buộc |
| --- | --- | --- |
| users | id, email VARCHAR(254), password_hash VARCHAR(255), display_name VARCHAR(150), role ENUM Student/Staff/Manager, active BOOL, department_id FK NULL | Email UNIQUE lowercase; hash dùng password_hash/verify PHP; không trả hash; department bắt buộc cho handler; role/scope seed không body |
| user_capabilities/scopes | user_id, capability, department_id nullable, scope_all BOOL | UNIQUE quan hệ; seed explicit dispatch/report/handler; scope toàn trường explicit không suy từ NULL |
| departments/categories | id, name VARCHAR(150), active BOOL | Seed PB-A/PB-B, Học vụ/CNTT/Chưa xác định là ví dụ giả lập không phòng ban thật; chưa xác định một category seed |
| requests | id, reference VARCHAR(32), student_id, category_id, title VARCHAR(150), description TEXT, status ENUM5, department_id NULL, assignee_id NULL, due_at NULL, created_at, updated_at, started_at NULL, resolved_at NULL, closed_at NULL, record_version INT | Reference UNIQUE server sinh; student từ session; title 1–150, description 1–4000 sau trim; version 1 khi tạo;+1 mỗi ghi; department/assignee hoặc cả NULL hoặc bộ hợp lệ |
| submissions | student_id, submission_key CHAR(36), payload_hash CHAR(64), request_id FK | UNIQUE(student_id, key); SHA-256 canonical payload đã chuẩn hóa; map cùng transaction tạo; không xóa khóa trong prototype |
| questions | id, request_id, content TEXT, created_by, created_at, answered_at NULL | Content 1–4000;1câu hỏi mở/request kiểm bằng khóa request; question lịch sử giữ lại |
| answers | id, question_id, content TEXT, created_by, created_at | question_id UNIQUE, content 1–4000, chủ SV, ghi STU-04 |
| progress_notes | id, request_id, content TEXT, visibility ENUM public/internal, created_by, created_at | Content 1–4000, visibility default internal |
| results | id, request_id, result_content TEXT, created_by, created_at | request_id UNIQUE, content 1–4000; chỉ DSP-09, sau Resolved bất biến trong baseline |
| feedback | id, request_id, result_id, student_id, score TINYINT, comment TEXT NULL, created_at, updated_at | request_id UNIQUE; score 1..5; comment<=4000; created_at lần đầu, updated_at lần sửa cuối |
| history | id, request_id, event_type VARCHAR(40), actor_id, occurred_at, old_values JSON NULL, new_values JSON NULL, visibility ENUM | Append only cùng transaction chính; chỉ phần public qua STU; không ghi password/session/secret |

Index:requests(student_id, updated_at, id), requests(assignee_id, status), requests(department_id, status), requests(created_at), requests(resolved_at), requests(status, due_at), history(request_id, occurred_at, id), feedback(updated_at). NULL hạn≠0; NULL owner=chưa giao; NULL trung bình=không mẫu.

Seed có SV-A/SV-B/SV-C, NV-A thuộc PB-A, NV-B thuộc PB-B, DP-All quyền dispatch all, QL-A report PB-A, QL-All report all và 1 inactive user. Mật khẩu test do môi trường seed cấp, không chép secret thật. Fixtures reset mỗi TC hoặc transaction rollback test để case này không đè case khác. Nếu dùng request10–20 làm fixtures thì ghi rõ ID giả lập, không coi là ID vận hành.

Đề xuất cfg:page_size 20, keyword 100, title 150, content 4000, email 254, password 256, idle 30 phút, absolute 8 giờ, login 5 fail/15 phút, email+IP, time UI Asia/Ho_Chi_Minh. Đây không là tham số đã được sponsor chốt; thay tham số phải cập nhật FR, TC và version.
