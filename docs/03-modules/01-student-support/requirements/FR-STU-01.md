### [FR-STU-01] Gửi yêu cầu hỗ trợ

**Mô tả**

Tạo yêu cầu có mã để trường tiếp nhận và sinh viên theo dõi. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- title phải trim, dài 1–150 ký tự Unicode. description phải trim, dài 1–4000. category_id là ID danh mục đang hoạt động, kể cả loại Chưa xác định. Không có upload trong baseline. **Kiểm bởi:** AC-STU-01-09, AC-STU-01-11.
- student_id lấy duy nhất từ phiên. Giá trị student_id/role do client gửi bị bỏ qua. Người gửi không chọn department_id, assignee_id, status hoặc các thời điểm hệ thống. **Kiểm bởi:** AC-STU-01-03, AC-STU-01-10.
- submission_key là UUID v4 của một ý định gửi. Client giữ khóa đến khi nhận kết quả. UNIQUE(student_id, submission_key). Máy chủ lưu hash của category_id, title và description sau chuẩn hóa. **Kiểm bởi:** AC-STU-01-04, AC-STU-01-05, AC-STU-01-11.
- Cùng khóa/cùng payload trả lại HTTP 200 và cùng request_id/reference, không tạo lịch sử thứ hai. Khóa mới tạo HTTP 201. Cùng khóa/khác payload trả 409 SUBMISSION_KEY_CONFLICT, không ghi đè. **Kiểm bởi:** AC-STU-01-04, AC-STU-01-05, AC-STU-01-06, AC-STU-01-07, AC-STU-01-11.
- Một transaction tạo request ở Received, record_version=1 và history CREATED công khai. department_id/assignee_id/due_at/started_at/resolved_at/closed_at đều NULL. created_at lấy từ đồng hồ máy chủ. Thất bại rollback tất cả. **Kiểm bởi:** AC-STU-01-01, AC-STU-01-08, AC-STU-01-10.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền sinh viên hoặc mã chống giả mạo yêu cầu (CSRF). **Kiểm bởi:** NFR-SEC-01.
- 422 dữ liệu gửi sai, giữ biểu mẫu. **Kiểm bởi:** AC-STU-01-02, AC-STU-01-09, AC-STU-01-11.
- 409 cùng khóa nhưng đổi nội dung, giữ hồ sơ đã tạo. **Kiểm bởi:** AC-STU-01-06.
- 500 hoàn tác hoặc mất phản hồi, chỉ thử lại cùng khóa. **Kiểm bởi:** AC-STU-01-05, AC-STU-01-08.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-STU-01-01:** Khi Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục. → Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.
- **AC-STU-01-02:** Khi Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. → Báo tại trường; không tạo yêu cầu hay lịch sử một phần.
- **AC-STU-01-03:** Khi Phiên SV-A gửi thêm student_id=SV-B. → Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.
- **AC-STU-01-04:** Khi Gửi lại cùng submission_key hai lần. → Trả cùng mã; tổng số yêu cầu tăng một, không hai.
- **AC-STU-01-05:** Khi Commit tạo xong nhưng client không nhận response; gửi lại cùng khóa/payload. → HTTP 200 cùng reference; DB 1 request và 1 event CREATED cho khóa.
- **AC-STU-01-06:** Khi Cùng student/key nhưng đổi description. → 409 SUBMISSION_KEY_CONFLICT; bản đã tạo không đổi.
- **AC-STU-01-07:** Khi Cùng nội dung nhưng hai UUID khác nhau. → Tạo 2 yêu cầu hợp lệ; không chống trùng theo nội dung để làm mất ý định mới.
- **AC-STU-01-08:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

- **AC-STU-01-09:** Khi Gửi riêng tiêu đề Unicode dài 150 và 151 ký tự, mô tả dài 4000 và 4001 ký tự, category 199 đã ngừng và category 103 Chưa xác định. → Chỉ chấp nhận khi không quá giới hạn và danh mục hoạt động. Báo lỗi tại trường sai, giữ dữ liệu nhập và không thêm hồ sơ/lịch sử ở lượt bị từ chối.
- **AC-STU-01-10:** Khi SV-A gửi UUID hợp lệ kèm student_id=102, role=Manager, department_id=201, assignee_id=111, status=Closed và các mốc do máy khách chỉ định. → Máy chủ lấy chủ từ phiên, tạo Received, phiên bản 1, trường trách nhiệm và hạn/mốc xử lý rỗng, giờ tạo từ máy chủ. Có đúng một submission và một CREATED công khai.
- **AC-STU-01-11:** Khi Gửi submission_key không đúng UUID v4. Sau đó gửi khóa hợp lệ với khoảng trắng đầu/cuối, rồi gửi lại cùng khóa với chuỗi đã bỏ khoảng trắng. → Khóa sai báo 422. Hai cách nhập tương đương trả 201 rồi 200 cùng mã, chỉ một submission/hồ sơ/CREATED.

**Ví dụ Edge Case**

Rollback giao dịch: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

**Expected Result**

500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi. Kiểm theo TC-STU-01-08; kết quả thực thi ban đầu là Not Run.

