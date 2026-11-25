### [FR-DSP-01] Xem hàng chờ điều phối

**Mô tả**

Nhận biết yêu cầu cần phân loại hoặc phân công. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Có quyền điều phối trong phạm vi được cấu hình.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Điều phối viên mở hàng chờ và chọn bộ lọc.
2. Máy chủ giới hạn phạm vi điều phối rồi lọc yêu cầu.
3. Hiển thị mã, loại, trạng thái, đơn vị, người phụ trách và hạn; cho mở chi tiết nghiệp vụ.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| category / status / department | Tùy chọn | Giá trị danh mục và trạng thái hợp lệ. |
| unassigned | Tùy chọn | Lọc chưa có người phụ trách. |
| page | Tùy chọn | Theo page_size=20. |

- Điều phối là quyền dispatch của nhân viên/quản lý, không là nhóm người dùng thứ tư trong proposal. Scope lấy từ quyền máy chủ. Điều phối toàn trường mới thấy yêu cầu chưa phân công. **Kiểm bởi:** AC-DSP-01-01, AC-DSP-01-03, AC-DSP-01-06.
- Bộ lọc category_id, department_id nguyên dương trong danh mục và phạm vi; status enum5; unassigned boolean; page>=1, 20 dòng/trang. Mặc định còn mở, mọi loại, cả chưa phân công. **Kiểm bởi:** AC-DSP-01-02, AC-DSP-01-05, AC-DSP-01-06.
- Tìm reference/title chuỗi con, keyword<=100. Sắp created_at tăng dần, id tăng dần để hồ sơ cũ không bị chìm. Tổng và danh sách cùng điều kiện. **Kiểm bởi:** AC-DSP-01-05.
- unassigned=true khi assignee_id IS NULL. Sau DSP-04 thành công hồ sơ ra khỏi tập unassigned; vẫn thuộc Tất cả nếu đúng scope. Hàng chờ không tự thay trách nhiệm/trạng thái. **Kiểm bởi:** AC-DSP-01-04.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền điều phối hoặc lọc ngoài phạm vi. **Kiểm bởi:** AC-DSP-01-03, AC-DSP-01-06.
- 422 lọc sai. **Kiểm bởi:** AC-DSP-01-02, AC-DSP-01-05, AC-DSP-01-06.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-01-01:** Khi Tạo yêu cầu Chưa xác định bằng STU-01. → Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.
- **AC-DSP-01-02:** Khi category_id không tồn tại. → Báo tham số sai; không áp dụng loại khác âm thầm.
- **AC-DSP-01-03:** Khi Sinh viên gọi trang/điểm truy cập hàng chờ. → Từ chối; không lộ danh sách toàn trường.
- **AC-DSP-01-04:** Khi Phân công yêu cầu rồi lọc unassigned=true. → Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.

- **AC-DSP-01-05:** Khi DP-All mở hàng chờ mặc định có hồ sơ mở/đóng, hai hồ sơ mở trùng created_at và 21 hồ sơ mở. Tìm theo phần mã hoặc tiêu đề rồi thử từ khóa 101 ký tự. → Mặc định chỉ mở, có cả chưa giao; sắp created_at/ID tăng. Trang tối đa 20 và total thống nhất. Tìm đúng tập nguồn; báo 422 khi từ khóa quá dài.
- **AC-DSP-01-06:** Khi DP-A thử lọc PB-B và đọc hàng chờ mặc định. DP-All xem cùng dữ liệu. Thử page=0, unassigned sai kiểu, status không hợp lệ và department_id=0. → DP-A không thấy hồ sơ ngoài PB-A hoặc chưa giao, lọc PB-B trả 403. DP-All thấy hồ sơ chưa giao. Các bộ lọc không hợp lệ trả 422.

**Ví dụ Edge Case**

Sau phân công: Phân công yêu cầu rồi lọc unassigned=true.

**Expected Result**

Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi. Kiểm theo TC-DSP-01-04; kết quả thực thi ban đầu là Not Run.

