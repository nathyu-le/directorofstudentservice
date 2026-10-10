### [FR-RPT-03] Thống kê thời gian xử lý

**Mô tả**

Đo thời gian từ gửi đến giải quyết bằng dữ liệu mẫu. Đặc tả triển khai prototype thuộc Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Quản lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Có quyền quản lý trong phạm vi báo cáo được cấu hình.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Quản lý mở báo cáo Thống kê thời gian xử lý và chọn các bộ lọc được chức năng hỗ trợ.
2. Máy chủ xác thực reporting scope, kiểm từng tham số; chốt as_of một lần cho response.
3. Máy chủ tạo tập nguồn và tính kết quả theo Business Rules dưới đây trong cùng snapshot đọc nhất quán.
4. Giao diện hiển thị scope, as_of, cỡ mẫu và chỉ tiêu; ghi rõ kỳ lọc theo resolved_at. Trung bình NULL hiển thị Không có dữ liệu.
5. Quản lý đối chiếu danh sách reference nguồn trong scope; không thay đổi hồ sơ từ báo cáo.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| department_id/category_id | Tùy chọn | ID nguyên dương, hợp lệ và trong reporting scope; omit=mọi đơn vị/loại trong scope. |
| from_date/to_date | Tùy chọn | Hai ngày YYYY-MM-DD phải đi cùng nhau, from<=to. Omit cả hai=mọi thời gian. Lọc theo resolved_at. |

- Chỉ quản lý có report capability và reporting scope. Sinh viên/handler không quyền report:403. Chọn đơn vị ngoài scope:403, không trả một phần. Scope áp dụng TRƯỚC mọi COUNT/AVG.
- Hồ sơ resolved_at trong kỳ. elapsed_hours=(resolved_at-created_at)/3600 gồm cả thời gian chờ bổ sung, không phải giờ công nhân viên. count=N, mean=sum(elapsed)/N, min, max; N=0 thì mean/min/max=null.
- Kỳ ngày theo múi giờ Asia/Ho_Chi_Minh, từ 00:00 from inclusive đến00:00 ngày sau to exclusive; đổi sang UTC trước query. Omit cả hai không lọc kỳ; gửi một ngày hoặc định dạng sai:422.
- Request, question, note và history join không được làm nhân bản dòng nguồn. Aggregate trên request_id/feedback_id duy nhất. Response trả filters, as_of, sample_count, metrics, source_references; không có nội dung sinh viên hoặc internal note.
- Chỉ số được tính từ seed/prototype; không là kết quả vận hành toàn trường. N=0 count=0 là số thật; average=null là chưa có mẫu, không đổi thành0.
- metrics={mean_elapsed_hours, min_elapsed_hours, max_elapsed_hours}; sample_count=N. Giá trị tính bằng giây/3600, không làm tròn trước tính mean; UI hiển thị 2 chữ số thập phân và nhãn giờ từ gửi đến giải quyết.

**Alternative / Error Flows**

- 401: phiên không hợp lệ, yêu cầu đăng nhập; không có số liệu trong response.
- 403: thiếu report capability hoặc chọn đơn vị ngoài reporting scope; không trả số liệu một phần.
- 422: sai hoặc dùng bộ lọc không hỗ trợ theo FR; hiển thị lỗi bộ lọc, không âm thầm thay kỳ.
- 500/mất mạng: hiển thị Không tải được báo cáo, cho thử tải lại GET; không thay dữ liệu nghiệp vụ.

**Acceptance Criteria**

- **AC-RPT-03-01:** Khi Hai hồ sơ có thời gian 2 giờ và 4 giờ; một hồ sơ còn mở. → Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại.
- **AC-RPT-03-02:** Khi from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **AC-RPT-03-03:** Khi Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **AC-RPT-03-04:** Khi Không hồ sơ đã giải quyết → Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ.
- **AC-RPT-03-05:** Khi Hai record có resolved_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. → Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.
- **AC-RPT-03-06:** Khi Một request có 3 notes,2 questions và 5 history events. → Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

**Ví dụ Edge Case**

Join không nhân bản: Một request có 3 notes,2 questions và 5 history events.

**Expected Result**

Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con. Kiểm theo TC-RPT-03-06; kết quả thực thi ban đầu là Not Run.
