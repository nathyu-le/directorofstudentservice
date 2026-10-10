### [FR-DSP-02] Xem hồ sơ xử lý và lịch sử nghiệp vụ

**Mô tả**

Đọc thông tin cần xử lý và truy vết thao tác theo trách nhiệm. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Điều phối viên / Nhân viên xử lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Có quyền trên yêu cầu theo vai trò và phạm vi công việc.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Người dùng mở hồ sơ từ hàng chờ hoặc danh sách được giao.
2. Máy chủ kiểm tra vai trò và phạm vi yêu cầu.
3. Hiển thị thông tin sinh viên tối thiểu phục vụ xử lý, nội dung yêu cầu, trách nhiệm và lịch sử có thứ tự.
4. Giao diện chỉ đưa thao tác phù hợp trạng thái và quyền; máy chủ vẫn kiểm tra khi thực hiện.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| request_id | Bắt buộc | Yêu cầu thuộc phạm vi được xem. |
| history | Chỉ đọc | Người, thời gian, hành động và thay đổi; phân biệt nội bộ/công khai. |

- Nhân viên xử lý chỉ đọc hồ sơ được giao cho chính mình. Điều phối đọc trong scope dispatch. Quản lý xem số liệu RPT; quyền đọc chi tiết cần được cấp riêng, không mặc định bởi vai trò.
- Đầu ra gồm nội dung hồ sơ, thông tin định danh mẫu của sinh viên cần cho xử lý, bộ trách nhiệm, hạn, version, câu hỏi/trả lời, kết quả và lịch sử public/internal đúng scope.
- Lịch sử append-only có event_id, event_type, actor_id, occurred_at, old/new values và visibility. Sắp occurred_at tăng dần, event_id tăng dần. Giao lại không xóa lịch sử.
- Phản hồi nội bộ không gửi sang STU. Không có endpoint tải tài liệu trong baseline; nếu bổ sung sau này phải tuân cùng quyền đối tượng và CR.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-02-01:** Khi Yêu cầu đã tiếp nhận, phân loại, phân công. → Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.
- **AC-DSP-02-02:** Khi Mở mã không tồn tại. → Không tìm thấy; không sinh dữ liệu.
- **AC-DSP-02-03:** Khi Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. → Từ chối, không trả nội dung sinh viên.
- **AC-DSP-02-04:** Khi Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. → Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.

**Ví dụ Edge Case**

Ghi chú nội bộ: Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03.

**Expected Result**

Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp. Kiểm theo TC-DSP-02-04; kết quả thực thi ban đầu là Not Run.
