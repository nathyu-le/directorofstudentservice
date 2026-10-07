# Quyết định kỹ thuật đề xuất

- ADR-01: PHP/MySQL monolith cho prototype — công nghệ từ proposal, giảm công việc ngoài phạm vi; phiên bản chốt lúc setup.
- ADR-02: transaction + record_version cho ghi nghiệp vụ/lịch sử — tránh ghi một phần và ghi đè; cần test rollback/đồng thời.
- ADR-03: account/department/category seed giả lập — đủ minh họa 4 module; không tự thêm module admin.
- ADR-04: phiên server và kiểm tra object-level permission — hạn chế truy cập đúng mục tiêu proposal; UI chỉ hỗ trợ người dùng.

Trạng thái: Proposed, chủ review BE với QA/PM; chưa coi đã được sponsor duyệt. Đổi quyết định phải nêu FR/test/Master ảnh hưởng.
