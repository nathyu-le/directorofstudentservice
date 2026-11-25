### [FR-DSP-05] Đặt hạn xử lý dự kiến

**Mô tả**

Cung cấp mốc theo dõi quá hạn cho yêu cầu; không tự áp SLA thực tế. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P1 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Điều phối viên chọn hạn dự kiến; nếu đổi hạn thì ghi lý do.
2. Kiểm tra hạn, quyền và phiên bản.
3. Lưu hạn và lịch sử thay đổi, không đổi trạng thái hoặc người phụ trách.
4. Chi tiết và báo cáo quá hạn sử dụng hạn hiện hành.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| due_at | Bắt buộc khi đặt hạn | Ngày giờ không trước created_at; cấu hình múi giờ Asia/Ho_Chi_Minh, lưu UTC. |
| reason | Khi đổi hạn | Lý do không rỗng. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ dispatch và Received/Processing/WaitingInfo. due_at bắt buộc, ISO8601 có timezone. So sánh UTC với created_at; cho đặt bằng created_at hoặc thời điểm quá khứ nếu không trước created_at để thể hiện backlog. **Kiểm bởi:** AC-DSP-05-01, AC-DSP-05-02, AC-DSP-05-03, AC-DSP-05-09, AC-DSP-05-10.
- Đổi hạn đã có cần reason sau khi bỏ khoảng trắng đầu/cuối, dài 1–4000. Cùng hạn trả 200 no_change. Transaction cập nhật due_at+version+history; không đổi owner/status. **Kiểm bởi:** AC-DSP-05-05, AC-DSP-05-06, AC-DSP-05-07, AC-DSP-05-08, AC-DSP-05-09, AC-DSP-05-10.
- Đây là hạn dự kiến do điều phối nhập, không SLA tự động. Prototype không có lịch ngày nghỉ nghiệp vụ, thời gian tạm dừng hoặc thông báo tự gửi. **Kiểm bởi:** AC-DSP-05-04, AC-DSP-05-09.
- Không hạn không tính quá hạn; now=due_at chưa quá hạn; now>due_at và trạng thái còn mở mới quá hạn. Resolved/Closed không xuất hiện trong RPT-02 dù resolved trễ. **Kiểm bởi:** AC-DSP-05-04, AC-DSP-05-10.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền điều phối hoặc CSRF. **Kiểm bởi:** AC-DSP-05-03, NFR-SEC-01.
- 404 ngoài phạm vi hồ sơ. **Kiểm bởi:** NFR-SEC-01.
- 422 hạn/lý do không hợp lệ. **Kiểm bởi:** AC-DSP-05-02, AC-DSP-05-06, AC-DSP-05-09, AC-DSP-05-10.
- 409 hồ sơ kết thúc hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-05-08, AC-DSP-05-10.
- 500 hoàn tác hạn và lịch sử. **Kiểm bởi:** AC-DSP-05-07.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-05-01:** Khi Hạn hai ngày sau created_at. → Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.
- **AC-DSP-05-02:** Khi due_at nhỏ hơn created_at. → Từ chối; hạn cũ giữ nguyên.
- **AC-DSP-05-03:** Khi Sinh viên hoặc nhân viên không điều phối sửa hạn. → Từ chối; không thay hạn.
- **AC-DSP-05-04:** Khi Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. → Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.
- **AC-DSP-05-05:** Khi Gửi lại cùng giá trị hiện hành với version mới nhất. → 200 no_change; version và số history không tăng.
- **AC-DSP-05-06:** Khi Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. → 422 reason; giá trị/version/history giữ nguyên.
- **AC-DSP-05-07:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-05-08:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-DSP-05-09:** Khi Trên hồ sơ 310 có created_at 2026-12-01T01:00:00Z, đặt hạn 2026-12-01T08:00:00+07:00, sau đó thử chuỗi không có múi giờ. → Lượt đầu lưu đúng 01:00Z và giữ owner/status; hạn thiếu múi giờ trả 422. Ghi thay đổi cùng version và lịch sử ở lượt hợp lệ.
- **AC-DSP-05-10:** Khi DP-All thử đặt hạn cho 313 Resolved và 314 Closed với giờ hợp lệ và phiên bản hiện hành. Trên hồ sơ mở thử đổi hạn với lý do 4001 ký tự. → Hai hồ sơ kết thúc trả 409, lý do dài trả 422. Dữ liệu/hạn/version/lịch sử giữ như trước.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-05-08; kết quả thực thi ban đầu là Not Run.

