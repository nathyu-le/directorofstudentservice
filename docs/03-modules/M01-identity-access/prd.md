# M01 Đặc tả Danh tính và quyền truy cập

## [FR-IAM-01] Đăng nhập

**Mô tả**

Xác thực tài khoản nạp sẵn và mở khu vực đúng vai trò.

**Actor**

SV, NV, QL

**Preconditions**

- Tài khoản đã được nạp vào dữ liệu mẫu; người dùng chưa có phiên hợp lệ.

**Luồng chính**

1. Người dùng mở trang đăng nhập và nhập email, mật khẩu.
2. Máy chủ kiểm tra dữ liệu, trạng thái tài khoản và mật khẩu đã băm.
3. Thành công, hệ thống đổi mã phiên và mở trang Yêu cầu của tôi, Công việc hoặc Hàng chờ theo vai trò.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| email | Chuỗi bắt buộc | Định dạng email; tối đa 254 ký tự; bỏ khoảng trắng hai đầu. |
| password | Chuỗi bắt buộc | Không cắt khoảng trắng; đối chiếu mật khẩu tài khoản mẫu. |
| session | Hệ thống tạo | Gắn user_id; hết hạn sau 30 phút không hoạt động. |

**Business Rules**

- Điều kiện nghiệp vụ: Tài khoản đã được nạp vào dữ liệu mẫu; người dùng chưa có phiên hợp lệ.
- Kết quả bắt buộc: Tạo phiên đăng nhập; không đổi dữ liệu Ticket.
- Quy tắc dùng chung: BR-01, BR-11. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Email sai định dạng hoặc trường trống: báo tại trường, không tạo phiên.
- Sai mật khẩu hoặc tài khoản ngừng hoạt động: thông báo chung Thông tin đăng nhập không hợp lệ.
- Lỗi máy chủ: cho phép thử lại; không tạo phiên xác thực một phần.

**Acceptance Criteria**

- **AC-IAM-01-01** Với tài khoản hoạt động của từng vai trò và mật khẩu đúng, tạo phiên mới và mở đúng khu vực.
- **AC-IAM-01-02** Email trống hoặc không hợp lệ bị từ chối và không tạo phiên.
- **AC-IAM-01-03** Sai thông tin hoặc tài khoản vô hiệu hóa đều trả cùng thông báo chung.
- **AC-IAM-01-04** Sau 30 phút không hoạt động, yêu cầu nghiệp vụ bị chặn; đăng nhập lại mới được tiếp tục.
- **AC-IAM-01-05** Mã phiên trước xác thực không được dùng để truy cập phiên sau xác thực.

**Ví dụ Edge Case**

Người dùng thử tài khoản vô hiệu hóa với mật khẩu đúng.

**Expected Result**

Không đăng nhập được; không tiết lộ lý do vô hiệu hóa.

**Hiệu ứng dữ liệu**

Tạo phiên đăng nhập; không đổi dữ liệu Ticket.

**Yêu cầu giao diện**

Email, mật khẩu, nút Đăng nhập; lỗi gần trường nhập.

**Liên kết thực hiện và kiểm thử**

Module M01; ưu tiên Must. Tiền đề triển khai: Không có FR tiền đề. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-IAM-01-01 đến TS-IAM-01-05.



## [FR-IAM-02] Đăng xuất

**Mô tả**

Kết thúc phiên hiện tại ở máy chủ.

**Actor**

SV, NV, QL

**Preconditions**

- Có phiên đăng nhập hợp lệ.

**Luồng chính**

1. Người dùng chọn Đăng xuất.
2. Máy chủ vô hiệu hóa phiên và xóa cookie phiên.
3. Hiển thị trang đăng nhập.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| session | Từ cookie phiên | Máy chủ xác định phiên; không nhận user_id tự khai. |

**Business Rules**

- Điều kiện nghiệp vụ: Có phiên đăng nhập hợp lệ.
- Kết quả bắt buộc: Phiên cũ mất hiệu lực.
- Quy tắc dùng chung: BR-01, BR-11. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Phiên đã hết hạn: trả về đăng nhập, không phát sinh lỗi nghiệp vụ.

**Acceptance Criteria**

- **AC-IAM-02-01** Đăng xuất thành công dẫn về trang đăng nhập.
- **AC-IAM-02-02** Gọi lại thao tác nghiệp vụ bằng phiên cũ bị từ chối.
- **AC-IAM-02-03** Back hoặc tải lại trang cũ không trả nội dung cần xác thực.
- **AC-IAM-02-04** Đăng nhập lại tạo phiên mới dùng được.

**Ví dụ Edge Case**

Một tab đã đăng xuất, tab khác còn form xử lý Ticket.

**Expected Result**

Tab còn lại không lưu được bằng phiên cũ; Ticket không đổi.

**Hiệu ứng dữ liệu**

Phiên cũ mất hiệu lực.

**Yêu cầu giao diện**

Nút Đăng xuất trong menu tài khoản.

**Liên kết thực hiện và kiểm thử**

Module M01; ưu tiên Must. Tiền đề triển khai: FR-IAM-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-IAM-02-01 đến TS-IAM-02-04.



## [FR-IAM-03] Kiểm soát quyền truy cập

**Mô tả**

Kiểm tra vai trò và quan hệ với Ticket tại máy chủ trước mọi lần đọc hoặc ghi.

**Actor**

Hệ thống

**Preconditions**

- Có tài khoản và cấu hình phòng ban, quyền quản lý trong dữ liệu mẫu.

**Luồng chính**

1. Máy chủ xác định actor từ phiên, không từ dữ liệu trình duyệt.
2. Kiểm quyền hành động và phạm vi Ticket theo ma trận quyền.
3. Chỉ truy vấn hoặc ghi khi hợp lệ; giao diện hiển thị các thao tác tương ứng.

**Dữ liệu và giới hạn**

| Trường | Kiểu và bắt buộc | Giới hạn hoặc nguồn |
| --- | --- | --- |
| role | SV, NV, QL | Lấy từ tài khoản hiện hành. |
| ticket_id | Khóa nếu thao tác trên Ticket | Kiểm chủ Ticket, phòng ban và người phụ trách hiện tại. |
| managed_departments | Tập khóa của QL | Cấu hình nạp sẵn; bộ lọc không mở rộng quyền. |

**Business Rules**

- Điều kiện nghiệp vụ: Có tài khoản và cấu hình phòng ban, quyền quản lý trong dữ liệu mẫu.
- Kết quả bắt buộc: Cho phép hoặc chặn thao tác; không tự đổi Ticket.
- Quy tắc dùng chung: BR-01, BR-02. Kiểm quyền, dữ liệu và điều kiện tại máy chủ trước ghi.

**Alternative / Error Flows**

- Không có phiên: yêu cầu đăng nhập.
- Sai vai trò: từ chối hành động.
- Ticket không tồn tại hoặc ngoài phạm vi: trả Không tìm thấy Ticket để tránh lộ thông tin.

**Acceptance Criteria**

- **AC-IAM-03-01** SV không đọc hoặc ghi Ticket của SV khác khi sửa URL hay tham số.
- **AC-IAM-03-02** NV thấy hàng chờ RECEIVED chưa phân công của phòng ban mình; chỉ xử lý Ticket đang giao cho mình.
- **AC-IAM-03-03** NV không đọc Ticket đã giao cho người khác hoặc đã chuyển sang phòng ban khác.
- **AC-IAM-03-04** QL chỉ tra cứu, điều phối và báo cáo trên các phòng ban được quản lý.
- **AC-IAM-03-05** Thay role, student_id hay department_id từ trình duyệt không cấp thêm quyền.
- **AC-IAM-03-06** Sau đổi người hoặc chuyển phòng ban, màn hình đang mở phải được kiểm quyền lại khi gửi.

**Ví dụ Edge Case**

NV cũ gửi thao tác hoàn tất sau khi QL đổi người phụ trách.

**Expected Result**

Bị từ chối theo quyền hiện tại; kết quả và trạng thái giữ nguyên.

**Hiệu ứng dữ liệu**

Cho phép hoặc chặn thao tác; không tự đổi Ticket.

**Yêu cầu giao diện**

Menu theo vai trò; trang từ chối không chứa nội dung Ticket.

**Liên kết thực hiện và kiểm thử**

Module M01; ưu tiên Must. Tiền đề triển khai: FR-IAM-01. Workflow: Kiểm riêng và tích hợp theo hành vi. Kịch bản: TS-IAM-03-01 đến TS-IAM-03-06.

[Về danh mục PRD](../../README.md) · [Ranh giới module](README.md) · [Quy tắc nghiệp vụ](../../02-domain/business-rules.md) · [Matrix](../../06-acceptance/traceability-matrix.md) · [Kịch bản kiểm thử](../../06-acceptance/test-scenarios.md)
