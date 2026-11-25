### [FR-IAM-01] Đăng nhập

**Mô tả**

Xác thực tài khoản mẫu trước khi sử dụng chức năng. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Sinh viên / Điều phối viên / Nhân viên / Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- email bắt buộc, trim, chuyển chữ thường, định dạng email, tối đa 254; password bắt buộc giữ nguyên không trim, tối đa 256. Tài khoản seed có password_hash, active, role/scope ở máy chủ. **Kiểm bởi:** AC-IAM-01-01, AC-IAM-01-07.
- Đúng email/password và active=true: regenerate session ID, hủy ID trước đăng nhập, trả user_id/display_name và capabilities hiện hành. Không nhận role/scope của client. **Kiểm bởi:** AC-IAM-01-01, AC-IAM-01-04, AC-IAM-01-08.
- Sai password/email không có/tài khoản inactive cùng trả 401 INVALID_CREDENTIALS và một thông báo chung. Đầu vào sai trả 422. Không log password/session token. Thời gian idle đề xuất 30 phút, absolute 8 giờ. **Kiểm bởi:** AC-IAM-01-02, AC-IAM-01-04, AC-IAM-01-05, AC-IAM-01-07, AC-IAM-01-08.
- Cookie HttpOnly, SameSite=Lax, Secure khi HTTPS. Local HTTP chỉ dùng môi trường mẫu, không được mô tả như bảo mật production. Login giới hạn đề xuất 5 lần sai/15 phút cho cặp email+IP:429; không là account-lock vĩnh viễn. **Kiểm bởi:** AC-IAM-01-06, AC-IAM-01-08.

**Alternative / Error Flows**

- 401 INVALID_CREDENTIALS dùng thông báo chung cho sai mật khẩu, email không có và inactive. **Kiểm bởi:** AC-IAM-01-02, AC-IAM-01-05.
- 422 email/mật khẩu không hợp lệ, không lặp mật khẩu trong phản hồi. **Kiểm bởi:** AC-IAM-01-07.
- 429 giới hạn thử sai cho cặp email/IP. **Kiểm bởi:** AC-IAM-01-06.
- 500 phản hồi lỗi đăng nhập có mã đối chiếu, không tự lập phiên trên máy khách. **Kiểm bởi:** NFR-OBS-01.

**Acceptance Criteria**

- **AC-IAM-01-01:** Khi Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập. → Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.
- **AC-IAM-01-02:** Khi Sai mật khẩu hoặc email không có. → Thông báo chung; không tạo phiên được phép nghiệp vụ.
- **AC-IAM-01-03:** Khi Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên. → Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.
- **AC-IAM-01-04:** Khi Dùng mã phiên trước đăng nhập hoặc phiên hết hạn idle 30 phút, absolute 8 giờ. → Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.
- **AC-IAM-01-05:** Khi Đúng password nhưng active=false. → 401 INVALID_CREDENTIALS như email không có; không cấp session.
- **AC-IAM-01-06:** Khi 5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP. → Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn.

- **AC-IAM-01-07:** Khi Dùng email seed với khoảng trắng đầu/cuối và chữ hoa. Gửi email sai định dạng hoặc 255 ký tự, mật khẩu rỗng hoặc 257 ký tự, rồi nối khoảng trắng vào mật khẩu seed hợp lệ. → Email đúng được chuẩn hóa. Dữ liệu sai báo 422. Mật khẩu có phần thêm bị đối chiếu nguyên văn và trả 401 nếu sai. Không ghi mật khẩu vào log.
- **AC-IAM-01-08:** Khi Trên cấu hình HTTPS thử đăng nhập đúng một actor seed, đọc thuộc tính cookie và gọi một điểm truy cập có bảo vệ sau mốc idle và sau mốc tuyệt đối. → Cookie HttpOnly, SameSite=Lax, Secure khi HTTPS. Phiên cũ đổi khi đăng nhập, các phiên quá hạn idle hoặc tuyệt đối bị từ chối.

**Ví dụ Edge Case**

Giới hạn thử sai: 5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP.

**Expected Result**

Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn. Kiểm theo TC-IAM-01-06; kết quả thực thi ban đầu là Not Run.

