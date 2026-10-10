### [FR-STU-02] Xem danh sách yêu cầu của tôi

**Mô tả**

Tìm yêu cầu mình đã gửi và biết trạng thái hiện tại. Đặc tả triển khai prototype thuộc Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Sinh viên mở Yêu cầu của tôi.
2. Máy chủ giới hạn theo user_id của phiên rồi áp dụng tìm kiếm, lọc và phân trang.
3. Trả mã, tiêu đề, trạng thái, đơn vị/người phụ trách nếu đã có, thời điểm cập nhật; cho mở chi tiết.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| keyword | Tùy chọn | Tìm mã hoặc tiêu đề; keyword tối đa 100 ký tự. |
| status | Tùy chọn | Một trạng thái hợp lệ hoặc Tất cả. |
| page | Tùy chọn | Số nguyên dương; kích thước trang page_size=20. |

- Truy vấn luôn có student_id=session.user_id trước mọi điều kiện tìm kiếm. Tham số student_id từ client không làm thay đổi phạm vi.
- keyword tùy chọn, trim, tối đa 100 ký tự, so khớp chuỗi con reference/title không phân biệt hoa thường. status tùy chọn thuộc Received/Processing/WaitingInfo/Resolved/Closed. page nguyên >=1; page_size cố định 20.
- Sắp updated_at giảm dần, id giảm dần khi bằng nhau. total và items dùng cùng scope/bộ lọc. Trang vượt số trang trả items rỗng, total đúng; không tự đổi trang.
- Mỗi dòng gồm reference, title, status, department_name, assignee_display_name, due_at, updated_at. NULL trách nhiệm hiển thị Chưa phân công; NULL hạn hiển thị Chưa đặt hạn. Không trả email riêng của nhân viên.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-STU-02-01:** Khi SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý. → Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.
- **AC-STU-02-02:** Khi page=0 hoặc trạng thái không thuộc danh sách. → Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.
- **AC-STU-02-03:** Khi SV-A sửa tham số student_id thành SV-B. → Không trả bất kỳ yêu cầu của SV-B.
- **AC-STU-02-04:** Khi Tài khoản SV-C chưa có yêu cầu. → Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.
- **AC-STU-02-05:** Khi Có 1 request, page=2 với page_size 20. → items=[] và total=1; không trả request người khác, không tự sửa page.

**Ví dụ Edge Case**

Trang vượt cuối: Có 1 request, page=2 với page_size 20.

**Expected Result**

items=[] và total=1; không trả request người khác, không tự sửa page. Kiểm theo TC-STU-02-05; kết quả thực thi ban đầu là Not Run.
