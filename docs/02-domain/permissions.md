# Ma trận quyền máy chủ

Ba nhóm người dùng theoproposal. Dispatch là quyền được cấp riêng trong seed cho mộtstaff/manager; có dispatch không tự có report và ngược lại.

| Nhóm / capability | Hành động | Scope đối tượng |
| --- | --- | --- |
| Student | STU-01..05 | Chủ request=self, chỉ public; không chọn chủ khi tạo |
| Staff có handler | DSP-02/06/07/08/09 | assignee_id=self hiện hành; không tự chiếm hồ sơ |
| Staff/Manager có dispatch | DSP-01/02/03/04/05/10 | Các department trongscope; chưa phân công chỉ scope toàn trường |
| Manager có report | RPT-01..06 | Aggregate trên reporting scope; chưa giao chỉ scope toàn trường |
| Mọi tài khoản | IAM-01/02 | Login/logout của chính phiên |
| Middleware | IAM-03 | Kiểm trước query/action trên máy chủ |

Thứ tự:session→active→capability→scope/object→state/version→validatepayload→transaction. 401thiếu phiên;403thiếu capability/CSRF hoặc chọn bộ lọc vượt scope;404object không có/ngoài object scope;422fieldsai;409state/version;500lỗi an toàn. Không trả 403kèm tên hồ sơ để lộ sự tồn tại. Không chấp nhận role/scope/student_id client để nâng quyền.

SV không nhận internal notes, actor/email riêng hoặc thông tin sinh viên khác trongHTML/JSON. Manager báo cáo không mặc định xem nội dung hồ sơ. Quyền được đọc lại mỗi request sauDSP-04. CSRF bắt buộc mọi mutation phiên hợp lệ. No-store nội dung nghiệp vụ. Tài liệu nếu CRbổ sung về sau áp dụng cùng scope, không có upload/tải file trong baseline.
