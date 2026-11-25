### [FR-DSP-04] Phân công trách nhiệm xử lý

**Mô tả**

Đặt một đơn vị và một người chịu trách nhiệm hiện tại, kể cả khi phối hợp đổi đơn vị. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Điều phối viên chọn đơn vị và nhân viên phụ trách.
2. Nếu đổi đơn vị/người, nhập lý do phối hợp và xác nhận.
3. Máy chủ kiểm tra quyền, quan hệ người–đơn vị và phiên bản.
4. Thay bộ trách nhiệm trong một giao dịch, giữ trạng thái và ghi người/đơn vị cũ–mới; quyền xử lý chuyển sang người mới.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| department_id | Bắt buộc | Đơn vị mẫu đang dùng. |
| assignee_id | Bắt buộc | Nhân viên hoạt động thuộc đơn vị. |
| reason | Khi đổi | Lý do đổi trách nhiệm không rỗng. |
| record_version | Hệ thống | Khóa kiểm soát cập nhật đồng thời. |

- Cho phép Received/Processing/WaitingInfo. department_id/assignee_id nguyên dương, đang hoạt động; assignee thuộc department và có quyền xử lý. **Kiểm bởi:** AC-DSP-04-01, AC-DSP-04-02, AC-DSP-04-03, AC-DSP-04-11.
- Đổi bộ trách nhiệm đã có cần reason trim 1–4000. Lần đầu không cần reason. Chọn cùng bộ hiện hành trả 200 no_change, không ghi sự kiện. **Kiểm bởi:** AC-DSP-04-01, AC-DSP-04-05, AC-DSP-04-06, AC-DSP-04-09, AC-DSP-04-11.
- Transaction khóa request, kiểm version, thay cả department+assignee, tăng version, ghi lịch sử old/new public. Một request chỉ có đúng một bộ trách nhiệm hiện hành. Chuyển phòng là thay bộ này, không sao chép hồ sơ. **Kiểm bởi:** AC-DSP-04-04, AC-DSP-04-07, AC-DSP-04-08, AC-DSP-04-09.
- Giữ status, due_at, question, result và mọi lịch sử cũ. NV cũ mất quyền trên các request mới ngay sau commit; request đang ghi cạnh tranh bị khóa/version chặn. Đang WaitingInfo thì câu hỏi chuyển theo hồ sơ cho NV mới. **Kiểm bởi:** AC-DSP-04-04, AC-DSP-04-09.
- Resolved/Closed:409. Không tự xóa người để đưa lại hàng chờ, không tự nhận/claim bằng tài khoản không dispatch. RPT-05 chỉ tính bộ hiện hành, không đếm hai phòng. **Kiểm bởi:** AC-DSP-04-03, AC-DSP-04-09, AC-DSP-04-10.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền điều phối hoặc CSRF. **Kiểm bởi:** AC-DSP-04-03, NFR-SEC-01.
- 404 ngoài phạm vi hồ sơ. **Kiểm bởi:** NFR-SEC-01.
- 422 đơn vị/người không hợp lệ hoặc thiếu lý do đổi. **Kiểm bởi:** AC-DSP-04-02, AC-DSP-04-06, AC-DSP-04-11.
- 409 hồ sơ kết thúc hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-04-08, AC-DSP-04-10.
- 500 hoàn tác cả hai trường trách nhiệm. **Kiểm bởi:** AC-DSP-04-07.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-04-01:** Khi Giao yêu cầu cho NV-A thuộc PB-A. → Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.
- **AC-DSP-04-02:** Khi Chọn PB-A với nhân viên PB-B. → Từ chối; không tạo trách nhiệm không nhất quán.
- **AC-DSP-04-03:** Khi Nhân viên không có quyền điều phối gửi assignee_id của mình. → Từ chối; không thay trách nhiệm.
- **AC-DSP-04-04:** Khi Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. → Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.
- **AC-DSP-04-05:** Khi Gửi lại cùng giá trị hiện hành với version mới nhất. → 200 no_change; version và số history không tăng.
- **AC-DSP-04-06:** Khi Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. → 422 reason; giá trị/version/history giữ nguyên.
- **AC-DSP-04-07:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-04-08:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-DSP-04-09:** Khi DP-All giao hồ sơ Received chưa có người mà bỏ reason. Sau đó chuyển hồ sơ WaitingInfo 311 sang PB-B/NV-B có lý do. → Lần đầu thành công. Khi chuyển, cùng ID, câu hỏi 401, hạn và lịch sử cũ. Quyền xử lý chuyển sang NV-B; nguồn RPT-05 chỉ có bộ trách nhiệm mới.
- **AC-DSP-04-10:** Khi Dùng hồ sơ 313 Resolved và 314 Closed, gửi bộ PB-B/NV-B hợp lệ, lý do đủ và phiên bản đúng. → 409 ở cả hai, không đổi phòng/người, status, result, version hay số sự kiện.
- **AC-DSP-04-11:** Khi Thử department_id=299, assignee_id=199, ID 0 hoặc nhân viên không có quyền xử lý. Sau đó thử lý do đổi dài 4001 ký tự. → 422 từng lượt, không có bộ trách nhiệm nửa hợp lệ và không thêm lịch sử.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-04-08; kết quả thực thi ban đầu là Not Run.

