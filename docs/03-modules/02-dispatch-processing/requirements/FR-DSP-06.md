### [FR-DSP-06] Bắt đầu xử lý yêu cầu

**Mô tả**

Xác nhận bắt đầu xử lý yêu cầu đã tiếp nhận. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- Chỉ nhân viên đang được giao, status=Received, trách nhiệm hợp lệ. Nhân viên không tự chuyển hồ sơ chưa giao hoặc tự đổi assignee. **Kiểm bởi:** AC-DSP-06-01, AC-DSP-06-03, AC-DSP-06-07.
- Transaction chuyển Received→Processing, set started_at=server_now lần đầu, tăng version, history STARTED public. Giữ due_at và chủ hồ sơ. **Kiểm bởi:** AC-DSP-06-01, AC-DSP-06-05, AC-DSP-06-07.
- Lần nhấn thứ hai/trạng thái khác/version cũ trả 409, không ghi sự kiện khác và không đặt lại started_at. Ngoài trách nhiệm trả 404. **Kiểm bởi:** AC-DSP-06-02, AC-DSP-06-03, AC-DSP-06-04, AC-DSP-06-06.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền xử lý hoặc CSRF. **Kiểm bởi:** NFR-SEC-01.
- 404 hồ sơ không giao cho actor. **Kiểm bởi:** AC-DSP-06-03, AC-DSP-06-07.
- 422 phiên bản đầu vào hoặc ID không hợp lệ. **Kiểm bởi:** NFR-SEC-01.
- 409 trạng thái/phiên bản xung đột hoặc bắt đầu lặp. **Kiểm bởi:** AC-DSP-06-02, AC-DSP-06-04, AC-DSP-06-06.
- 500 hoàn tác chuyển sang Processing. **Kiểm bởi:** AC-DSP-06-05.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-06-01:** Khi NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình. → Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.
- **AC-DSP-06-02:** Khi Bắt đầu hồ sơ Đã đóng. → Từ chối; không thay trạng thái và thời điểm.
- **AC-DSP-06-03:** Khi NV-B bắt đầu hồ sơ giao NV-A. → Từ chối; trách nhiệm giữ nguyên.
- **AC-DSP-06-04:** Khi Gửi lặp lần bắt đầu. → Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.
- **AC-DSP-06-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-06-06:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-DSP-06-07:** Khi NV-A thử bắt đầu hồ sơ 310 chưa phân công rồi kiểm lại hồ sơ hợp lệ đã bắt đầu. → Lượt không có quyền đối tượng không thay dữ liệu. Với hồ sơ hợp lệ, started_at lấy đồng hồ máy chủ lần đầu, giữ due_at và student_id.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-06-06; kết quả thực thi ban đầu là Not Run.

