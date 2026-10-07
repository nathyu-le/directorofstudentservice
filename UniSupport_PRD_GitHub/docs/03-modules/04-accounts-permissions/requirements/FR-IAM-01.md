# [FR-IAM-01] Đăng nhập

**Module:** Tài khoản và quyền truy cập

**Nguồn phạm vi:** Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Xác thực tài khoản mẫu trước khi sử dụng chức năng.

## Actor

Sinh viên / Điều phối viên / Nhân viên / Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| email | Bắt buộc | Định dạng email; tối đa CFG-07. |
| password | Bắt buộc | Không rỗng; không ghi log hay lưu dạng thô. |
| session | Hệ thống | Sinh phiên mới khi xác thực thành công; CFG-08. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Người dùng nhập email và mật khẩu.
2. Máy chủ kiểm tra đầu vào và đối chiếu mật khẩu băm của tài khoản đang dùng.
3. Nếu đúng, thay mã phiên và mở khu vực phù hợp quyền; nếu sai trả thông báo chung.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Sai thông tin: Sai mật khẩu hoặc email không có. → Thông báo chung; không tạo phiên được phép nghiệp vụ.
- Chưa xác thực: Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên. → Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.
- Cố định phiên / hết hạn: Dùng mã phiên trước đăng nhập hoặc phiên hết hạn CFG-08. → Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-IAM-01-01 | Đúng tài khoản | Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai. |
| AC-IAM-01-02 | Sai thông tin | Thông báo chung; không tạo phiên được phép nghiệp vụ. |
| AC-IAM-01-03 | Chưa xác thực | Yêu cầu xác thực; không trả dữ liệu nghiệp vụ. |
| AC-IAM-01-04 | Cố định phiên / hết hạn | Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-IAM-01-01 | AC-IAM-01-01 | Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập. |
| TC-IAM-01-02 | AC-IAM-01-02 | Sai mật khẩu hoặc email không có. |
| TC-IAM-01-03 | AC-IAM-01-03 | Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên. |
| TC-IAM-01-04 | AC-IAM-01-04 | Dùng mã phiên trước đăng nhập hoặc phiên hết hạn CFG-08. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.

Master dùng parent `FR-IAM-01 | Đăng nhập` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** Khởi tạo tài khoản giả lập và môi trường prototype.
