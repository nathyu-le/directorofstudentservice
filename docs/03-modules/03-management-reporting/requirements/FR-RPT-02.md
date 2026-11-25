### [FR-RPT-02] Theo dõi yêu cầu quá hạn

**Mô tả**

Nhận biết yêu cầu còn mở đã vượt hạn dự kiến. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P1 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Có quyền quản lý trong phạm vi báo cáo được cấu hình.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Quản lý mở báo cáo Theo dõi yêu cầu quá hạn và chọn các bộ lọc được chức năng hỗ trợ.
2. Máy chủ xác thực reporting scope, kiểm từng tham số; chốt as_of một lần cho response.
3. Máy chủ tạo tập nguồn và tính kết quả theo Business Rules dưới đây trong cùng snapshot đọc nhất quán.
4. Giao diện hiển thị scope, as_of, cỡ mẫu và chỉ tiêu; ghi rõ backlog hiện hành, không lọc theo ngày tạo. Tập nguồn rỗng hiển thị count=0 và danh sách rỗng.
5. Quản lý đối chiếu danh sách reference nguồn trong scope; không thay đổi hồ sơ từ báo cáo.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| department_id/category_id | Tùy chọn | ID nguyên dương, hợp lệ và trong reporting scope; omit=mọi đơn vị/loại trong scope. |
| as_of | Chỉ đọc | Một thời điểm UTC từ server, không từ trình duyệt. Không nhận kỳ ngày tạo. |

- Chỉ quản lý có report capability và reporting scope. Sinh viên/handler không quyền report:403. Chọn đơn vị ngoài scope:403, không trả một phần. Phạm vi quyền áp dụng trước mọi COUNT/AVG. **Kiểm bởi:** AC-RPT-02-03.
- Backlog HIỆN TẠI ở mọi ngày tạo: due_at NOT NULL, due_at<as_of và status Received/Processing/WaitingInfo. as_of lấy server_now một lần cho cả response. Không có from/to, tránh giấu hồ sơ cũ. overdue_hours=(as_of-due_at)/3600. **Kiểm bởi:** AC-RPT-02-01, AC-RPT-02-04, AC-RPT-02-05, AC-RPT-02-07.
- Không chấp nhận from_date/to_date trong endpoint này:422 UNSUPPORTED_FILTER. Không âm thầm lọc theo ngày tạo. **Kiểm bởi:** AC-RPT-02-02.
- Request, question, note và history join không được làm nhân bản dòng nguồn. Aggregate trên request_id/feedback_id duy nhất. Response trả filters, as_of, sample_count, metrics, source_references; không có nội dung sinh viên hoặc internal note. **Kiểm bởi:** AC-RPT-02-03, AC-RPT-02-06.
- Chỉ số được tính từ seed/prototype; không là kết quả vận hành toàn trường. N=0 count=0 là số thật; average=null là chưa có mẫu, không đổi thành 0. **Kiểm bởi:** AC-RPT-02-07.
- metrics={overdue_count}; sample_count=overdue_count. source_references là các dòng {reference, status, department_id, assignee_id, due_at, overdue_hours}; sắp due_at tăng dần rồi request_id tăng dần, không trả nội dung sinh viên. **Kiểm bởi:** AC-RPT-02-01, AC-RPT-02-07.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền báo cáo hoặc lọc ngoài phạm vi. **Kiểm bởi:** AC-RPT-02-03.
- 422 UNSUPPORTED_FILTER khi gửi ngày. **Kiểm bởi:** AC-RPT-02-02.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-RPT-02-01:** Khi Có một yêu cầu mở quá hạn, một đúng hạn, một không hạn, một Đã giải quyết quá hạn. → Chỉ yêu cầu mở quá hạn xuất hiện.
- **AC-RPT-02-02:** Khi Gửi from_date/to_date vào báo cáo backlog hiện hành. → HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.
- **AC-RPT-02-03:** Khi Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **AC-RPT-02-04:** Khi now bằng due_at → Yêu cầu chưa bị tính quá hạn tại đúng ranh giới.
- **AC-RPT-02-05:** Khi Một request từ tháng trước còn mở trong scope; due_at nhỏ hơn as_of. → Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.
- **AC-RPT-02-06:** Khi Một request có 3 notes, 2 questions và 5 history events. → Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

- **AC-RPT-02-07:** Khi Đo tập trống rồi đo ba hồ sơ quá hạn, hai hồ sơ cùng due_at. Thêm một hồ sơ Closed, một hồ sơ không hạn và một hồ sơ đúng as_of. → Tập trống có sample_count và overdue_count bằng 0. Ba dòng quá hạn sắp theo hạn rồi ID tăng, giờ quá hạn tính theo cùng một as_of. Không đưa ba hồ sơ đối chứng vào nguồn.

**Ví dụ Edge Case**

Join không nhân bản: Một request có 3 notes, 2 questions và 5 history events.

**Expected Result**

Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con. Kiểm theo TC-RPT-02-06; kết quả thực thi ban đầu là Not Run.

