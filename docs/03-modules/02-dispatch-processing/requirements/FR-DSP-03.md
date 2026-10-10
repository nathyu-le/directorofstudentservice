### [FR-DSP-03] Phân loại yêu cầu

**Mô tả**

Gán loại vấn đề để định tuyến và tổng hợp báo cáo. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

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

- Cho phép Received/Processing/WaitingInfo. category_id là danh mục hoạt động; Chưa xác định được dùng lúc tiếp nhận, không dùng làm loại đích của thao tác phân loại.
- reason trim 1–4000 bắt buộc khi đổi từ loại xác định sang loại khác. Gán lần đầu từ Chưa xác định không bắt buộc reason. Chọn lại cùng loại trả 200 no_change, không tăng version/lịch sử.
- Kiểm quyền điều phối, version; transaction cập nhật category+version+history CATEGORY_CHANGED. Không đổi department/assignee/status/due_at. RPT-04 đọc loại hiện hành.
- Resolved/Closed hoặc version cũ:409. ID ngừng dùng/không có hoặc thiếu reason cần thiết:422. Nhân viên không quyền dispatch:403.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-03-01:** Khi Yêu cầu Chưa xác định được gán Học vụ. → Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.
- **AC-DSP-03-02:** Khi Gửi loại đã ngừng dùng hoặc ID không có. → Từ chối; loại và lịch sử không thay đổi.
- **AC-DSP-03-03:** Khi Nhân viên thường hoặc sinh viên tự đổi loại. → Từ chối; loại hiện hành giữ nguyên.
- **AC-DSP-03-04:** Khi Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. → Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.
- **AC-DSP-03-05:** Khi Gửi lại cùng giá trị hiện hành với version mới nhất. → 200 no_change; version và số history không tăng.
- **AC-DSP-03-06:** Khi Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. → 422 reason; giá trị/version/history giữ nguyên.
- **AC-DSP-03-07:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-03-08:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-03-08; kết quả thực thi ban đầu là Not Run.
