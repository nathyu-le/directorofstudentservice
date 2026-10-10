### [FR-IAM-03] Kiểm soát quyền theo vai trò và hồ sơ

**Mô tả**

Chỉ cho truy cập thông tin và thao tác trong trách nhiệm được cấp. Đặc tả triển khai prototype thuộc Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Máy chủ cho mọi vai trò. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Mọi điểm truy cập nghiệp vụ xác thực phiên.
2. Kiểm tra vai trò cho hành động và phạm vi hồ sơ hoặc báo cáo.
3. Nếu cho phép mới đọc/ghi; nếu từ chối không trả nội dung nhạy cảm và không tạo tác dụng phụ.
4. Giao diện phản ánh quyền nhưng không thay kiểm tra máy chủ; thay trách nhiệm có hiệu lực ngay ở thao tác tiếp theo.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| user / role / scope | Hệ thống | Lấy từ phiên và tài khoản, không từ body hoặc URL tự khai. |
| object_id / action | Bắt buộc theo API | Kiểm tra cả quyền hành động và quyền trên đối tượng. |

- Áp dụng middleware trước mọi endpoint protected và mọi truy vấn dữ liệu; kiểm user active, session, capability, scope và quyền object từ DB, không từ UI/client.
- Không phiên/hết hạn401. Vai trò thiếu quyền hành động403. Request không tồn tại/ngoài quyền object404. Bộ lọc department vượt scope403. CSRF thiếu/sai403. Không chứa dữ liệu nghiệp vụ trong error.
- Sinh viên chỉ request.student_id=self và public; handler chỉ request.assignee_id=self; dispatch theo configured scope; manager chỉ aggregate theo reporting scope. Quyền dispatch có thể gắn staff/manager nhưng không tự cấp cho mọi staff.
- Thay responsibility ở DSP-04 làm object permission đổi ngay ở request tiếp theo. Không cache dài quyền từ lần login. Seed định nghĩa phạm vi đầy đủ; quyền xem hàng chờ chưa giao chỉ dispatch toàn trường.
- Server bỏ qua client role/scope/student_id, không mass-assign fields. Đầu ra chỉ định rõ trường được phép; query báo cáo phải lọc scope trước aggregate.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-IAM-03-01:** Khi SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi. → Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.
- **AC-IAM-03-02:** Khi Gửi role=manager hoặc scope=all trong dữ liệu. → Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.
- **AC-IAM-03-03:** Khi SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi. → Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.
- **AC-IAM-03-04:** Khi Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp. → NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.
- **AC-IAM-03-05:** Khi Đăng nhập hợp lệ rồi gửi POST protected không CSRF hoặc CSRF của phiên khác. → 403 CSRF_INVALID; không ghi dữ liệu. GET chỉ đọc không yêu cầu CSRF.
- **AC-IAM-03-06:** Khi QL-A chỉ PB-A; gọi report PB-B hoặc scope=all. → PB-B403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A.

**Ví dụ Edge Case**

Aggregate vượt scope: QL-A chỉ PB-A; gọi report PB-B hoặc scope=all.

**Expected Result**

PB-B403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A. Kiểm theo TC-IAM-03-06; kết quả thực thi ban đầu là Not Run.
