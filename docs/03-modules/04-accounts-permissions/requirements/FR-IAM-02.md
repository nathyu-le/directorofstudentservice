### [FR-IAM-02] Đăng xuất

**Mô tả**

Kết thúc phiên sử dụng hiện tại. Đặc tả triển khai prototype thuộc Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Mọi vai trò đã đăng nhập. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đã đăng nhập hợp lệ.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Người dùng chọn Đăng xuất.
2. Máy chủ vô hiệu phiên hiện tại và xóa thông tin phiên phía trình duyệt.
3. Chuyển về đăng nhập; mọi thao tác với phiên cũ phải bị chặn.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| session | Hệ thống | Phiên hiện tại trên máy chủ. |

- POST logout vô hiệu phiên hiện tại ở máy chủ, xóa cookie, trả 204. Chưa có phiên hoặc đã thoát cũng204 để thao tác lặp an toàn.
- Không vô hiệu các phiên khác của cùng tài khoản. Back/trang cache không cấp lại quyền: các API protected trả 401; nội dung nghiệp vụ cache private, no-store.
- Request thay đổi dữ liệu dùng CSRF token gắn phiên; logout dùng CSRF khi có phiên. Đăng nhập lại sinh session mới, phiên cũ không phục hồi.

**Alternative / Error Flows**

- Có phiên hợp lệ: kiểm CSRF, vô hiệu phiên và trả 204.
- Không có/đã hết phiên:204, không tác động phiên khác. CSRF sai khi có phiên:403.
- Lỗi máy chủ:500, UI không khẳng định server đã logout; API protected vẫn theo trạng thái phiên thật.

**Acceptance Criteria**

- **AC-IAM-02-01:** Khi Đăng nhập rồi chọn Đăng xuất. → Trở về đăng nhập; phiên máy chủ bị vô hiệu.
- **AC-IAM-02-02:** Khi Gọi đăng xuất khi đã thoát. → Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.
- **AC-IAM-02-03:** Khi Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. → Bị chặn; dữ liệu không thay đổi.
- **AC-IAM-02-04:** Khi Back trang trước rồi đăng nhập lại. → Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.

**Ví dụ Edge Case**

Back / đăng nhập lại: Back trang trước rồi đăng nhập lại.

**Expected Result**

Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ. Kiểm theo TC-IAM-02-04; kết quả thực thi ban đầu là Not Run.
