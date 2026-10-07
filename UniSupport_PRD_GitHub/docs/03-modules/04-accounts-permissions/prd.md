# PRD module: Tài khoản và quyền truy cập

Bản tổng hợp để review; file từng FR và requirements.json giữ cùng nội dung.

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


---

# [FR-IAM-02] Đăng xuất

**Module:** Tài khoản và quyền truy cập

**Nguồn phạm vi:** Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Kết thúc phiên sử dụng hiện tại.

## Actor

Mọi vai trò đã đăng nhập. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đã đăng nhập hợp lệ.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| session | Hệ thống | Phiên hiện tại trên máy chủ. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Người dùng chọn Đăng xuất.
2. Máy chủ vô hiệu phiên hiện tại và xóa thông tin phiên phía trình duyệt.
3. Chuyển về đăng nhập; mọi thao tác với phiên cũ phải bị chặn.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Không có phiên: Gọi đăng xuất khi đã thoát. → Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.
- Dùng lại phiên: Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. → Bị chặn; dữ liệu không thay đổi.
- Back / đăng nhập lại: Back trang trước rồi đăng nhập lại. → Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-IAM-02-01 | Đăng xuất đúng | Trở về đăng nhập; phiên máy chủ bị vô hiệu. |
| AC-IAM-02-02 | Không có phiên | Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác. |
| AC-IAM-02-03 | Dùng lại phiên | Bị chặn; dữ liệu không thay đổi. |
| AC-IAM-02-04 | Back / đăng nhập lại | Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-IAM-02-01 | AC-IAM-02-01 | Đăng nhập rồi chọn Đăng xuất. |
| TC-IAM-02-02 | AC-IAM-02-02 | Gọi đăng xuất khi đã thoát. |
| TC-IAM-02-03 | AC-IAM-02-03 | Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. |
| TC-IAM-02-04 | AC-IAM-02-04 | Back trang trước rồi đăng nhập lại. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Trở về đăng nhập; phiên máy chủ bị vô hiệu.

Master dùng parent `FR-IAM-02 | Đăng xuất` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-IAM-01


---

# [FR-IAM-03] Kiểm soát quyền theo vai trò và hồ sơ

**Module:** Tài khoản và quyền truy cập

**Nguồn phạm vi:** Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Chỉ cho truy cập thông tin và thao tác trong trách nhiệm được cấp.

## Actor

Máy chủ cho mọi vai trò. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| user / role / scope | Hệ thống | Lấy từ phiên và tài khoản, không từ body hoặc URL tự khai. |
| object_id / action | Bắt buộc theo API | Kiểm tra cả quyền hành động và quyền trên đối tượng. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Mọi điểm truy cập nghiệp vụ xác thực phiên.
2. Kiểm tra vai trò cho hành động và phạm vi hồ sơ hoặc báo cáo.
3. Nếu cho phép mới đọc/ghi; nếu từ chối không trả nội dung nhạy cảm và không tạo tác dụng phụ.
4. Giao diện phản ánh quyền nhưng không thay kiểm tra máy chủ; thay trách nhiệm có hiệu lực ngay ở thao tác tiếp theo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Vai trò giả: Gửi role=manager hoặc scope=all trong dữ liệu. → Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.
- Đối tượng của người khác: SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi. → Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.
- Thay trách nhiệm / dữ liệu nội bộ: Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp. → NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-IAM-03-01 | Quyền phù hợp | Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó. |
| AC-IAM-03-02 | Vai trò giả | Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt. |
| AC-IAM-03-03 | Đối tượng của người khác | Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan. |
| AC-IAM-03-04 | Thay trách nhiệm / dữ liệu nội bộ | NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-IAM-03-01 | AC-IAM-03-01 | SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi. |
| TC-IAM-03-02 | AC-IAM-03-02 | Gửi role=manager hoặc scope=all trong dữ liệu. |
| TC-IAM-03-03 | AC-IAM-03-03 | SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi. |
| TC-IAM-03-04 | AC-IAM-03-04 | Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.

Master dùng parent `FR-IAM-03 | Kiểm soát quyền theo vai trò và hồ sơ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-IAM-01
