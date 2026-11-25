### [FR-STU-04] Bổ sung thông tin được yêu cầu

**Mô tả**

Trả lời yêu cầu bổ sung trên cùng hồ sơ hỗ trợ. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- question_id nguyên dương thuộc cùng request_id; content trim 1–4000; record_version nguyên >=1 lấy từ màn hình hiện hành. **Kiểm bởi:** AC-STU-04-02, AC-STU-04-08, AC-STU-04-09.
- Khóa request trong transaction; đúng chủ, WaitingInfo, question chưa có answer, record_version khớp. Lưu answer gắn question_id duy nhất, đóng câu hỏi, chuyển Processing, tăng version và history công khai cùng commit. **Kiểm bởi:** AC-STU-04-01, AC-STU-04-04, AC-STU-04-06, AC-STU-04-07, AC-STU-04-09.
- Giữ request_id, student_id, department_id, assignee_id, due_at và started_at ban đầu. Đây là tiếp tục xử lý, không tạo Ticket khác và không đặt lại đồng hồ. **Kiểm bởi:** AC-STU-04-05.
- Sai chủ:404. Nội dung rỗng:422. Câu hỏi không thuộc request:422 QUESTION_INVALID. Phiên bản cũ/câu hỏi đã trả lời/trạng thái sai:409. Gửi hai lần chỉ lần đầu ghi dữ liệu; lần sau trả 409. **Kiểm bởi:** AC-STU-04-02, AC-STU-04-03, AC-STU-04-04, AC-STU-04-07, AC-STU-04-08, AC-STU-04-09.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền hành động hoặc CSRF. **Kiểm bởi:** NFR-SEC-01.
- 404 hồ sơ không thuộc người gửi. **Kiểm bởi:** AC-STU-04-03.
- 422 nội dung, phiên bản đầu vào hoặc câu hỏi không thuộc hồ sơ. **Kiểm bởi:** AC-STU-04-02, AC-STU-04-08, AC-STU-04-09.
- 409 hồ sơ cũ hoặc câu hỏi đã xử lý. **Kiểm bởi:** AC-STU-04-04, AC-STU-04-07.
- 500 lỗi giao dịch lưu câu trả lời. **Kiểm bởi:** AC-STU-04-06.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-STU-04-01:** Khi Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01. → Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.
- **AC-STU-04-02:** Khi Trả lời chỉ có khoảng trắng. → Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.
- **AC-STU-04-03:** Khi SV-B gửi câu trả lời cho yêu cầu SV-A. → Từ chối; nội dung và trạng thái không đổi.
- **AC-STU-04-04:** Khi Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. → Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.
- **AC-STU-04-05:** Khi Ghi lại started_at, due_at, owner trước khi trả lời question mở. → Sau bổ sung cả 3 giá trị giữ nguyên, Processing, một câu trả lời cho question.
- **AC-STU-04-06:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-STU-04-07:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-STU-04-08:** Khi SV-A lấy câu hỏi 402 thuộc hồ sơ 312 và gửi cho hồ sơ 311 đang WaitingInfo, kèm phiên bản đúng của 311. → 422 QUESTION_INVALID. Giữ nguyên status, version và câu hỏi của hai hồ sơ. Không tạo answer hoặc history.
- **AC-STU-04-09:** Khi Dùng câu hỏi 401 của hồ sơ 311. Gửi nội dung 4001 ký tự, record_version=0 rồi nội dung 4000 ký tự với phiên bản hiện hành. → Hai lượt sai báo 422 và giữ đầu vào. Lượt hợp lệ ghi đúng một answer, đóng câu hỏi, tăng version và ghi INFO_ANSWERED công khai.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-STU-04-07; kết quả thực thi ban đầu là Not Run.

