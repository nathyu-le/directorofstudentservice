### [FR-DSP-03] Phân loại yêu cầu

**Mô tả**

Gán loại vấn đề để định tuyến và tổng hợp báo cáo. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P1 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Điều phối viên mở hồ sơ và chọn loại vấn đề.
2. Máy chủ kiểm tra danh mục, quyền và phiên bản.
3. Lưu loại, lý do nếu đổi, lịch sử loại cũ/mới.
4. Không tự thay người hoặc phòng ban đang phụ trách; phân công qua DSP-04.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| category_id | Bắt buộc | Một loại đang dùng; không nhận ID giả. |
| reason | Khi đổi loại | Lý do không rỗng. |
| record_version | Hệ thống | Kiểm tra phiên bản hiện hành. |

- Cho phép Received/Processing/WaitingInfo. category_id là danh mục hoạt động; Chưa xác định được dùng lúc tiếp nhận, không dùng làm loại đích của thao tác phân loại. **Kiểm bởi:** AC-DSP-03-01, AC-DSP-03-02, AC-DSP-03-09.
- reason trim 1–4000 bắt buộc khi đổi từ loại xác định sang loại khác. Gán lần đầu từ Chưa xác định không bắt buộc reason. Chọn lại cùng loại trả 200 no_change, không tăng version/lịch sử. **Kiểm bởi:** AC-DSP-03-01, AC-DSP-03-05, AC-DSP-03-06, AC-DSP-03-09.
- Kiểm quyền điều phối, version; transaction cập nhật category+version+history CATEGORY_CHANGED. Không đổi department/assignee/status/due_at. RPT-04 đọc loại hiện hành. **Kiểm bởi:** AC-DSP-03-04, AC-DSP-03-07, AC-DSP-03-08, AC-DSP-03-09.
- Resolved/Closed hoặc version cũ:409. ID ngừng dùng/không có hoặc thiếu reason cần thiết:422. Nhân viên không quyền dispatch:403. **Kiểm bởi:** AC-DSP-03-02, AC-DSP-03-03, AC-DSP-03-06, AC-DSP-03-08, AC-DSP-03-09.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền điều phối hoặc CSRF. **Kiểm bởi:** AC-DSP-03-03, NFR-SEC-01.
- 404 ngoài phạm vi hồ sơ. **Kiểm bởi:** NFR-SEC-01.
- 422 loại/lý do không hợp lệ. **Kiểm bởi:** AC-DSP-03-02, AC-DSP-03-06, AC-DSP-03-09.
- 409 hồ sơ kết thúc hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-03-08, AC-DSP-03-09.
- 500 hoàn tác thay đổi phân loại. **Kiểm bởi:** AC-DSP-03-07.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-03-01:** Khi Yêu cầu Chưa xác định được gán Học vụ. → Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.
- **AC-DSP-03-02:** Khi Gửi loại đã ngừng dùng hoặc ID không có. → Từ chối; loại và lịch sử không thay đổi.
- **AC-DSP-03-03:** Khi Nhân viên thường hoặc sinh viên tự đổi loại. → Từ chối; loại hiện hành giữ nguyên.
- **AC-DSP-03-04:** Khi Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. → Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.
- **AC-DSP-03-05:** Khi Gửi lại cùng giá trị hiện hành với version mới nhất. → 200 no_change; version và số history không tăng.
- **AC-DSP-03-06:** Khi Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. → 422 reason; giá trị/version/history giữ nguyên.
- **AC-DSP-03-07:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-03-08:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-DSP-03-09:** Khi DP-All phân loại hồ sơ Resolved, Closed và hồ sơ Received với loại đích Chưa xác định. Thử đổi loại hợp lệ với lý do 4000 và 4001 ký tự. → Hồ sơ kết thúc trả 409, loại đích không hợp lệ hoặc lý do quá dài trả 422. Đổi hợp lệ ghi CATEGORY_CHANGED và giữ trách nhiệm, trạng thái, hạn.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-03-08; kết quả thực thi ban đầu là Not Run.

