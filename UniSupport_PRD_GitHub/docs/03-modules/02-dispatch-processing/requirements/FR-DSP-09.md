### [FR-DSP-09] Ghi kết quả giải quyết

**Mô tả**

Ghi kết quả để sinh viên nhận và phản hồi. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Nhân viên nhập kết quả và xác nhận Đã giải quyết.
2. Máy chủ kiểm tra quyền, trạng thái và câu hỏi bổ sung.
3. Lưu kết quả công khai, resolved_at, chuyển Đã giải quyết và ghi lịch sử cùng giao dịch.
4. Sinh viên thấy kết quả và có thể phản hồi qua STU-05.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| result_content | Bắt buộc | Kết quả/hướng dẫn không rỗng; 4000 ký tự. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ nhân viên được giao, Processing, không có question mở. result_content trim1–4000 bắt buộc.
- Transaction tạo result duy nhất/request, chuyển Processing→Resolved, set resolved_at=server_now, tăng version và history RESOLVED public. Kết quả được STU-03 đọc.
- Giữ started_at, due_at và trách nhiệm. Không đồng thời đóng request, không tự ghi feedback. Sau Resolved không có sửa/xóa kết quả trong baseline.
- WaitingInfo/Closed/Resolved hoặc version cũ trả 409. Hai lần giải quyết không tạo hai result; lỗi ghi history rollback cả result và status.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-09-01:** Khi Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận. → Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.
- **AC-DSP-09-02:** Khi Chờ bổ sung hoặc resolution rỗng. → Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.
- **AC-DSP-09-03:** Khi NV-B ghi kết quả cho hồ sơ NV-A. → Từ chối; không thay kết quả.
- **AC-DSP-09-04:** Khi Gửi lại cùng thao tác hoặc dùng record_version cũ. → Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.
- **AC-DSP-09-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-09-06:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.
- **AC-DSP-09-07:** Khi WaitingInfo có question mở; NV gửi result_content. → 409 INVALID_STATE; không result và không resolved_at mới.

**Ví dụ Edge Case**

Không giải quyết khi chờ: WaitingInfo có question mở; NV gửi result_content.

**Expected Result**

409 INVALID_STATE; không result và không resolved_at mới. Kiểm theo TC-DSP-09-07; kết quả thực thi ban đầu là Not Run.
