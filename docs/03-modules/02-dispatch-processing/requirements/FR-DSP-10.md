### [FR-DSP-10] Đóng yêu cầu đã giải quyết

**Mô tả**

Kết thúc hồ sơ đã có kết quả sau kiểm tra; bảo toàn lịch sử. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Điều phối viên kiểm tra kết quả và chọn Đóng yêu cầu.
2. Máy chủ kiểm tra quyền và trạng thái Đã giải quyết.
3. Chuyển Đã đóng, ghi closed_at và lịch sử; giữ kết quả, trách nhiệm và phản hồi.
4. Ngừng thao tác xử lý; vẫn cho người có quyền xem và sinh viên phản hồi kết quả.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ đã giải quyết. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ dispatch trong scope, status=Resolved và result tồn tại. record_version bắt buộc. Đóng là xác nhận kết thúc nghiệp vụ, không đợi một điểm hài lòng bắt buộc. **Kiểm bởi:** AC-DSP-10-01, AC-DSP-10-02, AC-DSP-10-03, AC-DSP-10-07.
- Transaction Resolved→Closed, set closed_at=server_now, tăng version và history CLOSED public. Không xóa kết quả hoặc dữ liệu. **Kiểm bởi:** AC-DSP-10-01, AC-DSP-10-05, AC-DSP-10-07.
- Closed chỉ đọc và STU-05 phản hồi/cập nhật điểm. Không phân loại/phân công/đặt hạn/bổ sung/tiến độ/giải quyết lại. Không tự đóng sau thời gian cố định. **Kiểm bởi:** AC-DSP-10-01, AC-DSP-10-04, AC-DSP-10-07.
- Gửi lặp/trạng thái trước Resolved/version cũ:409. Nhân viên xử lý không dispatch:403. Đây không là chức năng mở lại/hủy. **Kiểm bởi:** AC-DSP-10-02, AC-DSP-10-03, AC-DSP-10-04, AC-DSP-10-06.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền điều phối hoặc CSRF. **Kiểm bởi:** AC-DSP-10-03, NFR-SEC-01.
- 404 hồ sơ ngoài phạm vi. **Kiểm bởi:** NFR-SEC-01.
- 409 chưa giải quyết, thiếu kết quả, đã đóng hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-10-02, AC-DSP-10-04, AC-DSP-10-06, AC-DSP-10-07.
- 500 hoàn tác thao tác đóng. **Kiểm bởi:** AC-DSP-10-05.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-10-01:** Khi Điều phối viên đóng yêu cầu Đã giải quyết có kết quả. → Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.
- **AC-DSP-10-02:** Khi Hồ sơ Đang xử lý chưa có kết quả. → Từ chối; không bỏ qua bước giải quyết.
- **AC-DSP-10-03:** Khi Sinh viên hoặc nhân viên thường gọi thao tác đóng. → Từ chối; trạng thái không đổi.
- **AC-DSP-10-04:** Khi Gửi lại thao tác đóng; sau đó thử ghi tiến độ. → Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.
- **AC-DSP-10-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-10-06:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-DSP-10-07:** Khi DP-All đóng hồ sơ 313 Resolved có result nhưng chưa có feedback. Trên fixture âm độc lập thử Resolved thiếu result. → Hồ sơ hợp lệ đóng và giữ nguyên result/dữ liệu. Fixture thiếu kết quả bị chặn. Không tạo feedback tự động, không tăng version khi bị từ chối.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-10-06; kết quả thực thi ban đầu là Not Run.

