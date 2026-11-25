### [FR-DSP-02] Xem hồ sơ xử lý và lịch sử nghiệp vụ

**Mô tả**

Đọc thông tin cần xử lý và truy vết thao tác theo trách nhiệm. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Điều phối viên / Nhân viên xử lý. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- Nhân viên xử lý chỉ đọc hồ sơ được giao cho chính mình. Điều phối đọc trong scope dispatch. Quản lý xem số liệu RPT; quyền đọc chi tiết cần được cấp riêng, không mặc định bởi vai trò. **Kiểm bởi:** AC-DSP-02-03, AC-DSP-02-05.
- Đầu ra gồm nội dung hồ sơ, thông tin định danh mẫu của sinh viên cần cho xử lý, bộ trách nhiệm, hạn, version, câu hỏi/trả lời, kết quả và lịch sử public/internal đúng scope. **Kiểm bởi:** AC-DSP-02-01, AC-DSP-02-04, AC-DSP-02-05.
- Lịch sử append-only có event_id, event_type, actor_id, occurred_at, old/new values và visibility. Sắp occurred_at tăng dần, event_id tăng dần. Giao lại không xóa lịch sử. **Kiểm bởi:** AC-DSP-02-01, AC-DSP-02-05.
- Phản hồi nội bộ không gửi sang STU. Không có endpoint tải tài liệu trong baseline; nếu bổ sung sau này phải tuân cùng quyền đối tượng và CR. **Kiểm bởi:** AC-DSP-02-04.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền nghiệp vụ. **Kiểm bởi:** AC-DSP-02-05.
- 404 ID không có hoặc đối tượng ngoài phạm vi. **Kiểm bởi:** AC-DSP-02-02, AC-DSP-02-03.
- 422 ID sai định dạng. **Kiểm bởi:** AC-DSP-02-06.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-02-01:** Khi Yêu cầu đã tiếp nhận, phân loại, phân công. → Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.
- **AC-DSP-02-02:** Khi Mở mã không tồn tại. → Không tìm thấy; không sinh dữ liệu.
- **AC-DSP-02-03:** Khi Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. → Từ chối, không trả nội dung sinh viên.
- **AC-DSP-02-04:** Khi Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. → Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.

- **AC-DSP-02-05:** Khi QL-A mở hồ sơ PB-A khi chưa có quyền chi tiết. NV-A và DP-All đọc hồ sơ đã giao lại có hai sự kiện cùng giờ. → Quyền báo cáo riêng không cho đọc chi tiết. Người có quyền nhận đầy đủ trạng thái, version, bổ sung/kết quả và nội dung lịch sử đúng thứ tự giờ rồi ID. Lịch sử cũ giữ sau phân công.
- **AC-DSP-02-06:** Khi Gọi GET chi tiết với ID 0 hoặc chữ. → 422, không trả lịch sử hoặc thông tin sinh viên.

**Ví dụ Edge Case**

Ghi chú nội bộ: Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03.

**Expected Result**

Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp. Kiểm theo TC-DSP-02-04; kết quả thực thi ban đầu là Not Run.

