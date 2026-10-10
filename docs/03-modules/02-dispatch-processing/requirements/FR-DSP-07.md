### [FR-DSP-07] Yêu cầu sinh viên bổ sung thông tin

**Mô tả**

Nêu thông tin còn thiếu và đặt yêu cầu vào trạng thái chờ trả lời. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Nhân viên ghi rõ thông tin còn thiếu và chọn Yêu cầu bổ sung.
2. Máy chủ kiểm tra quyền, trạng thái và không có câu hỏi mở khác.
3. Lưu câu hỏi, chuyển Chờ bổ sung và thêm sự kiện công khai.
4. Sinh viên thấy câu hỏi trong STU-03 và trả lời qua STU-04.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| question | Bắt buộc | Câu hỏi cụ thể không rỗng; 4000 ký tự. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ nhân viên được giao, Processing, chưa có question mở. question trim1–4000 phải nêu thông tin cần bổ sung.
- Transaction tạo question gắn request, visibility public, chưa có answer; chuyển Processing→WaitingInfo, tăng version và history INFO_REQUESTED.
- Một question mở tối đa/request được bảo vệ bởi khóa request khi ghi. Câu hỏi đã trả lời được giữ lịch sử; sau trở lại Processing có thể hỏi đợt mới với question_id khác.
- Không tự đặt lại due_at/started_at. Không gửi mail/notification ngoài baseline. Gửi lặp hoặc WaitingInfo:409; lỗi database rollback cả question và status.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-07-01:** Khi Đang xử lý, hỏi Vui lòng cung cấp mã lớp. → Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.
- **AC-DSP-07-02:** Khi Nội dung chỉ có khoảng trắng. → Không lưu câu hỏi hoặc chuyển trạng thái.
- **AC-DSP-07-03:** Khi NV-B yêu cầu bổ sung hồ sơ giao NV-A. → Từ chối; không lộ thêm dữ liệu.
- **AC-DSP-07-04:** Khi Gửi lần thứ hai khi Chờ bổ sung. → HTTP 409 INVALID_STATE; vẫn1question mở, không có history/question thứ hai.
- **AC-DSP-07-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-07-06:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.
- **AC-DSP-07-07:** Khi Sau STU-04 đóng question1 và Processing, NV hỏi question2. → Có question2 ID khác,1 question mở; question1/answer cũ giữ nguyên.

**Ví dụ Edge Case**

Đợt bổ sung tiếp theo: Sau STU-04 đóng question1 và Processing, NV hỏi question2.

**Expected Result**

Có question2 ID khác,1 question mở; question1/answer cũ giữ nguyên. Kiểm theo TC-DSP-07-07; kết quả thực thi ban đầu là Not Run.
