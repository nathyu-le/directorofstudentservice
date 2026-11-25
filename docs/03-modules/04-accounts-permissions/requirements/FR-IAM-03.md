### [FR-IAM-03] Kiểm soát quyền theo vai trò và hồ sơ

**Mô tả**

Chỉ cho truy cập thông tin và thao tác trong trách nhiệm được cấp. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Máy chủ cho mọi vai trò. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- Áp dụng middleware trước mọi endpoint protected và mọi truy vấn dữ liệu; kiểm user active, session, capability, scope và quyền object từ DB, không từ UI/client. **Kiểm bởi:** AC-IAM-03-01, AC-IAM-03-02, AC-IAM-03-05, AC-IAM-03-07.
- Không phiên/hết hạn trả 401. Vai trò thiếu quyền hành động trả 403. Request không tồn tại/ngoài quyền đối tượng trả 404. Bộ lọc department vượt phạm vi trả 403. CSRF thiếu/sai trả 403. Không chứa dữ liệu nghiệp vụ trong error. **Kiểm bởi:** AC-IAM-03-02, AC-IAM-03-03, AC-IAM-03-05, AC-IAM-03-06, AC-IAM-03-07, AC-IAM-03-08.
- Sinh viên chỉ request.student_id=self và public; handler chỉ request.assignee_id=self; dispatch theo configured scope; manager chỉ aggregate theo reporting scope. Quyền dispatch có thể gắn staff/manager nhưng không tự cấp cho mọi staff. **Kiểm bởi:** AC-IAM-03-01, AC-IAM-03-03, AC-IAM-03-06, AC-IAM-03-08.
- Thay responsibility ở DSP-04 làm object permission đổi ngay ở request tiếp theo. Không cache dài quyền từ lần login. Seed định nghĩa phạm vi đầy đủ; quyền xem hàng chờ chưa giao chỉ dispatch toàn trường. **Kiểm bởi:** AC-IAM-03-04, AC-IAM-03-08.
- Server bỏ qua client role/scope/student_id, không mass-assign fields. Đầu ra chỉ định rõ trường được phép; query báo cáo phải lọc scope trước aggregate. **Kiểm bởi:** AC-IAM-03-02, AC-IAM-03-03, AC-IAM-03-04, AC-IAM-03-06.

**Alternative / Error Flows**

- 401 phiên thiếu/hết hạn/tài khoản không hoạt động. **Kiểm bởi:** AC-IAM-03-07.
- 403 thiếu quyền, lọc ngoài phạm vi hoặc CSRF sai. **Kiểm bởi:** AC-IAM-03-02, AC-IAM-03-05, AC-IAM-03-06.
- 404 đối tượng không có hoặc không có quyền. **Kiểm bởi:** AC-IAM-03-03, AC-IAM-03-04, AC-IAM-03-08.
- 500 lỗi tra phiên/quyền, từ chối và không trả dữ liệu. **Kiểm bởi:** NFR-OBS-01.

**Acceptance Criteria**

- **AC-IAM-03-01:** Khi SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi. → Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.
- **AC-IAM-03-02:** Khi Gửi role=manager hoặc scope=all trong dữ liệu. → Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.
- **AC-IAM-03-03:** Khi SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi. → Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.
- **AC-IAM-03-04:** Khi Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp. → NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.
- **AC-IAM-03-05:** Khi Đăng nhập hợp lệ rồi gửi POST protected không CSRF hoặc CSRF của phiên khác. → 403 CSRF_INVALID; không ghi dữ liệu. GET chỉ đọc không yêu cầu CSRF.
- **AC-IAM-03-06:** Khi QL-A chỉ PB-A; gọi report PB-B hoặc scope=all. → PB-B trả 403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A.

- **AC-IAM-03-07:** Khi Tạo phiên hợp lệ, sau đó đổi tài khoản seed sang không hoạt động ở DB hoặc đưa đồng hồ quá giới hạn phiên. Dùng cookie cũ gọi GET và POST hợp lệ. → 401 và không đọc/ghi dữ liệu. Middleware lấy trạng thái hiện hành từ máy chủ.
- **AC-IAM-03-08:** Khi Dùng DP-A và DP-All đọc hàng chờ có hồ sơ chưa giao. Dùng SV-B gọi chi tiết hồ sơ SV-A và một ID hợp lệ không tồn tại. → Chỉ DP-All thấy chưa giao. Hai phản hồi 404 cho SV-B có nội dung tương đương, không tiết lộ hồ sơ có tồn tại.

**Ví dụ Edge Case**

Aggregate vượt scope: QL-A chỉ PB-A; gọi report PB-B hoặc scope=all.

**Expected Result**

PB-B trả 403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A. Kiểm theo TC-IAM-03-06; kết quả thực thi ban đầu là Not Run.

