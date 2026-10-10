### [FR-STU-04] Bổ sung thông tin được yêu cầu

**Mô tả**

Trả lời yêu cầu bổ sung trên cùng hồ sơ hỗ trợ. Đặc tả triển khai prototype thuộc Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Sinh viên đọc câu hỏi và nhập nội dung trả lời.
2. Máy chủ kiểm tra chủ hồ sơ, trạng thái, câu hỏi và phiên bản.
3. Lưu câu trả lời vào cùng request_id, đánh dấu câu hỏi đã được trả lời và chuyển về Đang xử lý.
4. Giữ người phụ trách; thêm lịch sử công khai để bên xử lý tiếp tục.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| question_id | Bắt buộc | Câu hỏi đang mở thuộc yêu cầu. |
| content | Bắt buộc | Nội dung không rỗng; 4000 ký tự. |
| record_version | Hệ thống | Phiên bản dùng khi gửi để phát hiện dữ liệu đã đổi. |

- question_id nguyên dương thuộc cùng request_id; content trim 1–4000; record_version nguyên >=1 lấy từ màn hình hiện hành.
- Khóa request trong transaction; đúng chủ, WaitingInfo, question chưa có answer, record_version khớp. Lưu answer gắn question_id duy nhất, đóng câu hỏi, chuyển Processing, tăng version và history công khai cùng commit.
- Giữ request_id, student_id, department_id, assignee_id, due_at và started_at ban đầu. Đây là tiếp tục xử lý, không tạo Ticket khác và không đặt lại đồng hồ.
- Sai chủ:404. Nội dung rỗng:422. Câu hỏi không thuộc request:422 QUESTION_INVALID. Phiên bản cũ/câu hỏi đã trả lời/trạng thái sai:409. Gửi hai lần chỉ lần đầu ghi dữ liệu; lần sau409.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-STU-04-01:** Khi Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01. → Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.
- **AC-STU-04-02:** Khi Trả lời chỉ có khoảng trắng. → Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.
- **AC-STU-04-03:** Khi SV-B gửi câu trả lời cho yêu cầu SV-A. → Từ chối; nội dung và trạng thái không đổi.
- **AC-STU-04-04:** Khi Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. → Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.
- **AC-STU-04-05:** Khi Ghi lại started_at, due_at, owner trước khi trả lời question mở. → Sau bổ sung cả 3 giá trị giữ nguyên, Processing,1answer cho question.
- **AC-STU-04-06:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-STU-04-07:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-STU-04-07; kết quả thực thi ban đầu là Not Run.
