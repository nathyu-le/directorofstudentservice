### [FR-DSP-06] Bắt đầu xử lý yêu cầu

**Mô tả**

Xác nhận bắt đầu xử lý yêu cầu đã tiếp nhận. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Nhân viên mở yêu cầu được giao và chọn Bắt đầu xử lý.
2. Máy chủ kiểm tra trách nhiệm và trạng thái.
3. Chuyển Đã tiếp nhận → Đang xử lý, ghi thời điểm bắt đầu và lịch sử công khai.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ được giao cho người thao tác. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ nhân viên đang được giao, status=Received, trách nhiệm hợp lệ. Nhân viên không tự chuyển hồ sơ chưa giao hoặc tự đổi assignee.
- Transaction chuyển Received→Processing, set started_at=server_now lần đầu, tăng version, history STARTED public. Giữ due_at và chủ hồ sơ.
- Lần nhấn thứ hai/trạng thái khác/version cũ trả 409, không ghi sự kiện khác và không đặt lại started_at. Ngoài trách nhiệm trả 404.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-06-01:** Khi NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình. → Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.
- **AC-DSP-06-02:** Khi Bắt đầu hồ sơ Đã đóng. → Từ chối; không thay trạng thái và thời điểm.
- **AC-DSP-06-03:** Khi NV-B bắt đầu hồ sơ giao NV-A. → Từ chối; trách nhiệm giữ nguyên.
- **AC-DSP-06-04:** Khi Gửi lặp lần bắt đầu. → Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.
- **AC-DSP-06-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-06-06:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-06-06; kết quả thực thi ban đầu là Not Run.
