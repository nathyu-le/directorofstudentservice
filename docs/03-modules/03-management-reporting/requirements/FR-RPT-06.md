### [FR-RPT-06] Tổng hợp mức hài lòng

**Mô tả**

Tổng hợp phản hồi kết quả để đánh giá chất lượng hỗ trợ. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P2 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Có quyền quản lý trong phạm vi báo cáo được cấu hình.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Quản lý mở báo cáo Tổng hợp mức hài lòng và chọn các bộ lọc được chức năng hỗ trợ.
2. Máy chủ xác thực reporting scope, kiểm từng tham số; chốt as_of một lần cho response.
3. Máy chủ tạo tập nguồn và tính kết quả theo Business Rules dưới đây trong cùng snapshot đọc nhất quán.
4. Giao diện hiển thị scope, as_of, cỡ mẫu và chỉ tiêu; ghi rõ kỳ lọc theo feedback.updated_at. Trung bình NULL hiển thị Không có dữ liệu.
5. Quản lý đối chiếu danh sách reference nguồn trong scope; không thay đổi hồ sơ từ báo cáo.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| department_id/category_id | Tùy chọn | ID nguyên dương, hợp lệ và trong reporting scope; omit=mọi đơn vị/loại trong scope. |
| from_date/to_date | Tùy chọn | Hai ngày YYYY-MM-DD phải đi cùng nhau, from<=to. Omit cả hai=mọi thời gian. Lọc theo feedback.updated_at. |

- Chỉ quản lý có report capability và reporting scope. Sinh viên/handler không quyền report:403. Chọn đơn vị ngoài scope:403, không trả một phần. Phạm vi quyền áp dụng trước mọi COUNT/AVG. **Kiểm bởi:** AC-RPT-06-03.
- Feedback hiện hành có updated_at trong kỳ. Một request một feedback; N số feedback, mean=sum(score)/N, phân bố 5 mức. Chưa đánh giá loại khỏi mẫu, không tính0. N=0 mean=null. **Kiểm bởi:** AC-RPT-06-01, AC-RPT-06-04, AC-RPT-06-05, AC-RPT-06-07, AC-RPT-06-08.
- Kỳ ngày theo múi giờ Asia/Ho_Chi_Minh, từ 00:00 from inclusive đến00:00 ngày sau to exclusive; đổi sang UTC trước query. Omit cả hai không lọc kỳ; gửi một ngày hoặc định dạng sai:422. **Kiểm bởi:** AC-RPT-06-02, AC-RPT-06-05.
- Request, question, note và history join không được làm nhân bản dòng nguồn. Aggregate trên request_id/feedback_id duy nhất. Response trả filters, as_of, sample_count, metrics, source_references; không có nội dung sinh viên hoặc internal note. **Kiểm bởi:** AC-RPT-06-03, AC-RPT-06-06.
- Chỉ số được tính từ seed/prototype; không là kết quả vận hành toàn trường. N=0 count=0 là số thật; average=null là chưa có mẫu, không đổi thành 0. **Kiểm bởi:** AC-RPT-06-04, AC-RPT-06-08.
- metrics={mean_score, score_counts:{1,2,3,4,5}}; sample_count=N. Tổng score_counts=N; mean_score hiển thị 2 chữ số thập phân, điểm chưa đánh giá không tính 0. **Kiểm bởi:** AC-RPT-06-01, AC-RPT-06-04, AC-RPT-06-08.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền báo cáo hoặc lọc ngoài phạm vi. **Kiểm bởi:** AC-RPT-06-03.
- 422 kỳ hoặc bộ lọc không hợp lệ. **Kiểm bởi:** AC-RPT-06-02.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-RPT-06-01:** Khi Có điểm 5 và 3; yêu cầu chưa đánh giá không tính. → Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.
- **AC-RPT-06-02:** Khi from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **AC-RPT-06-03:** Khi Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **AC-RPT-06-04:** Khi Chưa có phản hồi hoặc đổi điểm → Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.
- **AC-RPT-06-05:** Khi Hai record có feedback.updated_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. → Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.
- **AC-RPT-06-06:** Khi Một request có 3 notes, 2 questions và 5 history events. → Chỉ tính feedback hiện hành của request đó một lần, không nhân bản bởi dữ liệu con.
- **AC-RPT-06-07:** Khi Feedback4 tại kỳA đổi thành3 tại kỳB không trùng A. → Cùng feedback_id/request_id; kỳA không còn dòng này, kỳB có điểm3; mẫu toàn thời gian vẫn1.

- **AC-RPT-06-08:** Khi Tạo phản hồi 1 và 5, một hồ sơ không có phản hồi, rồi lọc bộ dữ liệu không có mẫu. → Mẫu là 2, mean_score=3.00, có đủ năm nhóm điểm, tổng nhóm là 2. Không tính hồ sơ chưa đánh giá. Không mẫu thì mean_score=null.

**Ví dụ Edge Case**

Sửa điểm và kỳ cập nhật: Feedback4 tại kỳA đổi thành3 tại kỳB không trùng A.

**Expected Result**

Cùng feedback_id/request_id; kỳA không còn dòng này, kỳB có điểm3; mẫu toàn thời gian vẫn1. Kiểm theo TC-RPT-06-07; kết quả thực thi ban đầu là Not Run.

