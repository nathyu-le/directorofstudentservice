### [FR-STU-01] Gửi yêu cầu hỗ trợ

**Mô tả**

Tạo yêu cầu có mã để trường tiếp nhận và sinh viên theo dõi. Đặc tả triển khai prototype thuộc Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Sinh viên mở Tạo yêu cầu. Client sinh submission_key cho ý định gửi và giữ khi retry.
2. Sinh viên chọn loại và nhập title, description, rồi chọn Gửi yêu cầu.
3. Máy chủ kiểm phiên/CSRF và chuẩn hóa/validate dữ liệu.
4. Máy chủ kiểm key/payload; replay trả reference cũ, conflict trả 409.
5. Nếu là ý định mới, transaction tạo request và CREATED theo Business Rules.
6. Máy chủ trả 201 reference; UI mở tiến độ và hiển thị thành công. Hàng chờ đọc được cùng request sau commit.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| category_id | Bắt buộc | Loại vấn đề đang dùng; cho phép giá trị Chưa xác định để điều phối phân loại. |
| title | Bắt buộc | Văn bản không chỉ có khoảng trắng; tối đa 150 ký tự. |
| description | Bắt buộc | Nội dung vấn đề không rỗng; tối đa 4000 ký tự. |
| submission_key | Hệ thống | Mã của lần gửi để nhận biết gửi lặp cùng thao tác. |

- title phải trim, dài 1–150 ký tự Unicode. description phải trim, dài 1–4000. category_id là ID danh mục đang hoạt động, kể cả loại Chưa xác định. Không có upload trong baseline.
- student_id lấy duy nhất từ phiên. Giá trị student_id/role do client gửi bị bỏ qua. Người gửi không chọn department_id, assignee_id, status hoặc các thời điểm hệ thống.
- submission_key là UUID v4 của một ý định gửi. Client giữ khóa đến khi nhận kết quả. UNIQUE(student_id, submission_key). Máy chủ lưu hash của category_id, title và description sau chuẩn hóa.
- Cùng khóa/cùng payload trả lại HTTP 200 và cùng request_id/reference, không tạo lịch sử thứ hai. Khóa mới tạo HTTP 201. Cùng khóa/khác payload trả 409 SUBMISSION_KEY_CONFLICT, không ghi đè.
- Một transaction tạo request ở Received, record_version=1 và history CREATED công khai. department_id/assignee_id/due_at/started_at/resolved_at/closed_at đều NULL. created_at lấy từ đồng hồ máy chủ. Thất bại rollback tất cả.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-STU-01-01:** Khi Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục. → Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.
- **AC-STU-01-02:** Khi Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. → Báo tại trường; không tạo yêu cầu hay lịch sử một phần.
- **AC-STU-01-03:** Khi Phiên SV-A gửi thêm student_id=SV-B. → Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.
- **AC-STU-01-04:** Khi Gửi lại cùng submission_key hai lần. → Trả cùng mã; tổng số yêu cầu tăng một, không hai.
- **AC-STU-01-05:** Khi Commit tạo xong nhưng client không nhận response; gửi lại cùng khóa/payload. → HTTP 200 cùng reference; DB 1 request và 1 event CREATED cho khóa.
- **AC-STU-01-06:** Khi Cùng student/key nhưng đổi description. → 409 SUBMISSION_KEY_CONFLICT; bản đã tạo không đổi.
- **AC-STU-01-07:** Khi Cùng nội dung nhưng hai UUID khác nhau. → Tạo2 yêu cầu hợp lệ; không chống trùng theo nội dung để làm mất ý định mới.
- **AC-STU-01-08:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Ví dụ Edge Case**

Rollback giao dịch: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

**Expected Result**

500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi. Kiểm theo TC-STU-01-08; kết quả thực thi ban đầu là Not Run.
