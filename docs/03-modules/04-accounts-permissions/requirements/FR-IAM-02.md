### [FR-IAM-02] Đăng xuất

**Mô tả**

Kết thúc phiên sử dụng hiện tại. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Mọi vai trò đã đăng nhập. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- POST logout vô hiệu phiên hiện tại ở máy chủ, xóa cookie, trả 204. Chưa có phiên hoặc đã thoát cũng trả 204 để thao tác lặp an toàn. **Kiểm bởi:** AC-IAM-02-01, AC-IAM-02-02, AC-IAM-02-03.
- Không vô hiệu các phiên khác của cùng tài khoản. Back/trang cache không cấp lại quyền: các API protected trả 401; nội dung nghiệp vụ cache private, no-store. **Kiểm bởi:** AC-IAM-02-03, AC-IAM-02-04, AC-IAM-02-05.
- Request thay đổi dữ liệu dùng CSRF token gắn phiên; logout dùng CSRF khi có phiên. Đăng nhập lại sinh session mới, phiên cũ không phục hồi. **Kiểm bởi:** AC-IAM-02-04, AC-IAM-02-05.

**Alternative / Error Flows**

- 204 trả khi không có phiên hoặc đã thoát, API cũ vẫn bị từ chối. **Kiểm bởi:** AC-IAM-02-02, AC-IAM-02-03.
- 403 khi có phiên mà CSRF sai hoặc thiếu. **Kiểm bởi:** AC-IAM-02-05.
- 500 khi xử lý phiên thất bại, giao diện không xác nhận đã thoát. **Kiểm bởi:** AC-IAM-02-06.

**Acceptance Criteria**

- **AC-IAM-02-01:** Khi Đăng nhập rồi chọn Đăng xuất. → Trở về đăng nhập; phiên máy chủ bị vô hiệu.
- **AC-IAM-02-02:** Khi Gọi đăng xuất khi đã thoát. → Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.
- **AC-IAM-02-03:** Khi Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. → Bị chặn; dữ liệu không thay đổi.
- **AC-IAM-02-04:** Khi Back trang trước rồi đăng nhập lại. → Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.

- **AC-IAM-02-05:** Khi Mở hai phiên độc lập của SV-A. Đăng xuất phiên A với token thiếu/sai rồi với token đúng. Thử đọc hồ sơ bằng cả hai phiên. → Token sai trả 403, A còn hiệu lực. Token đúng trả 204, A bị vô hiệu và xóa cookie, B vẫn có quyền. Kiểm header private, no-store ở màn hình dữ liệu.
- **AC-IAM-02-06:** Khi Giả lập lỗi lưu trạng thái phiên trước khi xác nhận logout ở môi trường test, rồi thử lại bình thường. → Lượt lỗi trả 500 có mã đối chiếu, không báo đã thoát. Lượt sau trả 204 và phiên bị vô hiệu, không lộ thông tin trong lỗi.

**Ví dụ Edge Case**

Back / đăng nhập lại: Back trang trước rồi đăng nhập lại.

**Expected Result**

Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ. Kiểm theo TC-IAM-02-04; kết quả thực thi ban đầu là Not Run.

