### [FR-RPT-01] Tổng hợp trạng thái yêu cầu

**Mô tả**

Đếm yêu cầu theo trạng thái để theo dõi tổng thể. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Có quyền quản lý trong phạm vi báo cáo được cấu hình.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Quản lý mở báo cáo Tổng hợp trạng thái yêu cầu và chọn các bộ lọc được chức năng hỗ trợ.
2. Máy chủ xác thực reporting scope, kiểm từng tham số; chốt as_of một lần cho response.
3. Máy chủ tạo tập nguồn và tính kết quả theo Business Rules dưới đây trong cùng snapshot đọc nhất quán.
4. Giao diện hiển thị scope, as_of, cỡ mẫu và chỉ tiêu; ghi rõ kỳ lọc theo created_at. Tập nguồn rỗng hiển thị count=0 và danh sách rỗng.
5. Quản lý đối chiếu danh sách reference nguồn trong scope; không thay đổi hồ sơ từ báo cáo.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| department_id/category_id | Tùy chọn | ID nguyên dương, hợp lệ và trong reporting scope; omit=mọi đơn vị/loại trong scope. |
| from_date/to_date | Tùy chọn | Hai ngày YYYY-MM-DD phải đi cùng nhau, from<=to. Omit cả hai=mọi thời gian. Lọc theo created_at. |

- Chỉ quản lý có report capability và reporting scope. Sinh viên/handler không quyền report:403. Chọn đơn vị ngoài scope:403, không trả một phần. Phạm vi quyền áp dụng trước mọi COUNT/AVG. **Kiểm bởi:** AC-RPT-01-03.
- Tập yêu cầu được tạo trong kỳ; nhóm theo status HIỆN TẠI. Không khôi phục trạng thái lịch sử cuối kỳ. Đủ 5 nhóm kể cả nhóm bằng 0; tổng nhóm=N. **Kiểm bởi:** AC-RPT-01-01, AC-RPT-01-05, AC-RPT-01-07.
- Kỳ ngày theo múi giờ Asia/Ho_Chi_Minh, từ 00:00 from inclusive đến00:00 ngày sau to exclusive; đổi sang UTC trước query. Omit cả hai không lọc kỳ; gửi một ngày hoặc định dạng sai:422. **Kiểm bởi:** AC-RPT-01-02, AC-RPT-01-05.
- Request, question, note và history join không được làm nhân bản dòng nguồn. Aggregate trên request_id/feedback_id duy nhất. Response trả filters, as_of, sample_count, metrics, source_references; không có nội dung sinh viên hoặc internal note. **Kiểm bởi:** AC-RPT-01-03, AC-RPT-01-06.
- Chỉ số được tính từ seed/prototype; không là kết quả vận hành toàn trường. N=0 count=0 là số thật; average=null là chưa có mẫu, không đổi thành 0. **Kiểm bởi:** AC-RPT-01-04.
- metrics={total_requests, counts:{Received, Processing, WaitingInfo, Resolved, Closed}}; sample_count=total_requests. **Kiểm bởi:** AC-RPT-01-01, AC-RPT-01-04, AC-RPT-01-07.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền báo cáo hoặc lọc ngoài phạm vi. **Kiểm bởi:** AC-RPT-01-03.
- 422 kỳ hoặc bộ lọc không hợp lệ. **Kiểm bởi:** AC-RPT-01-02.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-RPT-01-01:** Khi Tạo 10 yêu cầu: 2 mỗi trạng thái. → Mỗi nhóm bằng 2, tổng bằng 10.
- **AC-RPT-01-02:** Khi from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **AC-RPT-01-03:** Khi Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **AC-RPT-01-04:** Khi Không có yêu cầu → Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng.
- **AC-RPT-01-05:** Khi Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. → Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.
- **AC-RPT-01-06:** Khi Một request có 3 notes, 2 questions và 5 history events. → Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

- **AC-RPT-01-07:** Khi Tạo hồ sơ trong kỳ, chuyển Received sang Processing sau cuối kỳ rồi đo kỳ cũ. → Hồ sơ được nhóm theo Processing hiện tại, có đủ năm nhóm, nhóm không có nguồn đếm 0, tổng đúng mẫu.

**Ví dụ Edge Case**

Join không nhân bản: Một request có 3 notes, 2 questions và 5 history events.

**Expected Result**

Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con. Kiểm theo TC-RPT-01-06; kết quả thực thi ban đầu là Not Run.

