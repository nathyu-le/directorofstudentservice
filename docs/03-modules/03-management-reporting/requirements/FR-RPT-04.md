### [FR-RPT-04] Thống kê vấn đề phổ biến

**Mô tả**

Biết nhóm vấn đề nào có nhiều yêu cầu. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P2 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Có quyền quản lý trong phạm vi báo cáo được cấu hình.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Quản lý mở báo cáo Thống kê vấn đề phổ biến và chọn các bộ lọc được chức năng hỗ trợ.
2. Máy chủ xác thực reporting scope, kiểm từng tham số; chốt as_of một lần cho response.
3. Máy chủ tạo tập nguồn và tính kết quả theo Business Rules dưới đây trong cùng snapshot đọc nhất quán.
4. Giao diện hiển thị scope, as_of, cỡ mẫu và chỉ tiêu; ghi rõ kỳ lọc theo created_at. Tập nguồn rỗng hiển thị count=0 và danh sách rỗng.
5. Quản lý đối chiếu danh sách reference nguồn trong scope; không thay đổi hồ sơ từ báo cáo.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| department_id/category_id | Tùy chọn | ID nguyên dương, hợp lệ và trong reporting scope; omit=mọi đơn vị/loại trong scope. |
| from_date/to_date | Tùy chọn | Hai ngày YYYY-MM-DD phải đi cùng nhau, from<=to. Omit cả hai=mọi thời gian. Lọc theo created_at. |

- Chỉ quản lý có report capability và reporting scope. Sinh viên/handler không quyền report:403. Chọn đơn vị ngoài scope:403, không trả một phần. Phạm vi quyền áp dụng trước mọi COUNT/AVG. **Kiểm bởi:** AC-RPT-04-03.
- Yêu cầu được tạo trong kỳ nhóm theo category HIỆN TẠI, một request tính một lần. Nhóm Chưa xác định không bị bỏ. Count giảm dần rồi category_id tăng dần khi hòa. **Kiểm bởi:** AC-RPT-04-01, AC-RPT-04-04, AC-RPT-04-05, AC-RPT-04-07.
- Kỳ ngày theo múi giờ Asia/Ho_Chi_Minh, từ 00:00 from inclusive đến00:00 ngày sau to exclusive; đổi sang UTC trước query. Omit cả hai không lọc kỳ; gửi một ngày hoặc định dạng sai:422. **Kiểm bởi:** AC-RPT-04-02, AC-RPT-04-05.
- Request, question, note và history join không được làm nhân bản dòng nguồn. Aggregate trên request_id/feedback_id duy nhất. Response trả filters, as_of, sample_count, metrics, source_references; không có nội dung sinh viên hoặc internal note. **Kiểm bởi:** AC-RPT-04-03, AC-RPT-04-06.
- Chỉ số được tính từ seed/prototype; không là kết quả vận hành toàn trường. N=0 count=0 là số thật; average=null là chưa có mẫu, không đổi thành 0. **Kiểm bởi:** AC-RPT-04-07.
- metrics={groups:[{category_id, category_name, count}], total_requests}; sample_count=total_requests. Tổng group.count bằng sample_count. **Kiểm bởi:** AC-RPT-04-01, AC-RPT-04-07.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền báo cáo hoặc lọc ngoài phạm vi. **Kiểm bởi:** AC-RPT-04-03.
- 422 kỳ hoặc bộ lọc không hợp lệ. **Kiểm bởi:** AC-RPT-04-02.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-RPT-04-01:** Khi Ba Học vụ, hai CNTT và một Chưa xác định. → Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần.
- **AC-RPT-04-02:** Khi from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **AC-RPT-04-03:** Khi Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **AC-RPT-04-04:** Khi Hai nhóm bằng số lượng → Nhóm count bằng nhau sắp category_id tăng dần; không bỏ Chưa xác định.
- **AC-RPT-04-05:** Khi Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. → Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.
- **AC-RPT-04-06:** Khi Một request có 3 notes, 2 questions và 5 history events. → Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

- **AC-RPT-04-07:** Khi Tạo ba hồ sơ trong kỳ; một hồ sơ đổi loại sau cuối kỳ và hồ sơ khác giữ Chưa xác định. Đo lại kỳ cũ. → Nhóm theo danh mục hiện hành, vẫn tính Chưa xác định. Tổng nhóm bằng sample_count.

**Ví dụ Edge Case**

Join không nhân bản: Một request có 3 notes, 2 questions và 5 history events.

**Expected Result**

Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con. Kiểm theo TC-RPT-04-06; kết quả thực thi ban đầu là Not Run.

