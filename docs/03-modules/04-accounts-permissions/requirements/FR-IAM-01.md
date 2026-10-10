### [FR-IAM-01] Đăng nhập

**Mô tả**

Xác thực tài khoản mẫu trước khi sử dụng chức năng. Đặc tả triển khai prototype thuộc Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Sinh viên / Điều phối viên / Nhân viên / Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Người dùng nhập email và mật khẩu.
2. Máy chủ kiểm tra đầu vào và đối chiếu mật khẩu băm của tài khoản đang dùng.
3. Nếu đúng, thay mã phiên và mở khu vực phù hợp quyền; nếu sai trả thông báo chung.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| email | Bắt buộc | Định dạng email; tối đa 254 ký tự. |
| password | Bắt buộc | Không rỗng; không ghi log hay lưu dạng thô. |
| session | Hệ thống | Sinh phiên mới khi xác thực thành công; idle 30 phút, absolute 8 giờ. |

- email bắt buộc, trim, chuyển chữ thường, định dạng email, max254; password bắt buộc giữ nguyên không trim, max256. Tài khoản seed có password_hash, active, role/scope ở máy chủ.
- Đúng email/password và active=true: regenerate session ID, hủy ID trước đăng nhập, trả user_id/display_name và capabilities hiện hành. Không nhận role/scope của client.
- Sai password/email không có/tài khoản inactive cùng401 INVALID_CREDENTIALS và một thông báo chung. Đầu vào sai422. Không log password/session token. Thời gian idle đề xuất30 phút, absolute 8 giờ.
- Cookie HttpOnly, SameSite=Lax, Secure khi HTTPS. Local HTTP chỉ dùng môi trường mẫu, không được mô tả như bảo mật production. Login giới hạn đề xuất5 lần sai/15 phút cho cặp email+IP:429; không là account-lock vĩnh viễn.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-IAM-01-01:** Khi Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập. → Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.
- **AC-IAM-01-02:** Khi Sai mật khẩu hoặc email không có. → Thông báo chung; không tạo phiên được phép nghiệp vụ.
- **AC-IAM-01-03:** Khi Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên. → Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.
- **AC-IAM-01-04:** Khi Dùng mã phiên trước đăng nhập hoặc phiên hết hạn idle 30 phút, absolute 8 giờ. → Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.
- **AC-IAM-01-05:** Khi Đúng password nhưng active=false. → 401 INVALID_CREDENTIALS như email không có; không cấp session.
- **AC-IAM-01-06:** Khi 5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP. → Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn.

**Ví dụ Edge Case**

Giới hạn thử sai: 5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP.

**Expected Result**

Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn. Kiểm theo TC-IAM-01-06; kết quả thực thi ban đầu là Not Run.
