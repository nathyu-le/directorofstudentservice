### [FR-STU-02] Xem danh sách yêu cầu của tôi

**Mô tả**

Tìm yêu cầu mình đã gửi và biết trạng thái hiện tại. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- Truy vấn luôn có student_id=session.user_id trước mọi điều kiện tìm kiếm. Tham số student_id từ client không làm thay đổi phạm vi. **Kiểm bởi:** AC-STU-02-01, AC-STU-02-03, AC-STU-02-06.
- keyword tùy chọn, trim, tối đa 100 ký tự, so khớp chuỗi con reference/title không phân biệt hoa thường. status tùy chọn thuộc Received/Processing/WaitingInfo/Resolved/Closed. page nguyên >=1; page_size cố định 20. **Kiểm bởi:** AC-STU-02-02, AC-STU-02-06, AC-STU-02-07.
- Sắp updated_at giảm dần, id giảm dần khi bằng nhau. total và items dùng cùng scope/bộ lọc. Trang vượt số trang trả items rỗng, total đúng; không tự đổi trang. **Kiểm bởi:** AC-STU-02-05, AC-STU-02-07.
- Mỗi dòng gồm reference, title, status, department_name, assignee_display_name, due_at, updated_at. NULL trách nhiệm hiển thị Chưa phân công; NULL hạn hiển thị Chưa đặt hạn. Không trả email riêng của nhân viên. **Kiểm bởi:** AC-STU-02-04, AC-STU-02-07.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền đọc danh sách sinh viên. **Kiểm bởi:** NFR-SEC-01.
- 422 trạng thái, trang hoặc từ khóa sai. **Kiểm bởi:** AC-STU-02-02, AC-STU-02-06.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-STU-02-01:** Khi SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý. → Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.
- **AC-STU-02-02:** Khi page=0 hoặc trạng thái không thuộc danh sách. → Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.
- **AC-STU-02-03:** Khi SV-A sửa tham số student_id thành SV-B. → Không trả bất kỳ yêu cầu của SV-B.
- **AC-STU-02-04:** Khi Tài khoản SV-C chưa có yêu cầu. → Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.
- **AC-STU-02-05:** Khi Có 1 request, page=2 với page_size 20. → items=[] và total=1; không trả request người khác, không tự sửa page.

- **AC-STU-02-06:** Khi Với SV-A, tìm từ khóa có khoảng trắng đầu/cuối và khác chữ hoa/thường, khớp một phần mã US-310 hoặc tiêu đề Học vụ. Thử từ khóa 100 và 101 ký tự. → Tìm trên mã/tiêu đề đúng phạm vi. 100 ký tự hợp lệ, 101 trả 422 tại bộ lọc. Không đưa hồ sơ SV-B vào items/total.
- **AC-STU-02-07:** Khi Tạo 21 hồ sơ của SV-A. Hai hồ sơ mới nhất trùng updated_at nhưng ID khác. Một hồ sơ không có người/hạn. Đọc trang 1 và 2. → 20 dòng rồi 1 dòng, cùng total=21, thứ tự updated_at và ID giảm dần. Hiện Chưa phân công, Chưa đặt hạn. Không có email riêng nhân viên trong JSON.

**Ví dụ Edge Case**

Trang vượt cuối: Có 1 request, page=2 với page_size 20.

**Expected Result**

items=[] và total=1; không trả request người khác, không tự sửa page. Kiểm theo TC-STU-02-05; kết quả thực thi ban đầu là Not Run.

