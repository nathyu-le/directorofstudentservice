### [FR-DSP-04] Phân công trách nhiệm xử lý

**Mô tả**

Đặt một đơn vị và một người chịu trách nhiệm hiện tại, kể cả khi phối hợp đổi đơn vị. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

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

- Cho phép Received/Processing/WaitingInfo. department_id/assignee_id nguyên dương, đang hoạt động; assignee thuộc department và có quyền xử lý.
- Đổi bộ trách nhiệm đã có cần reason trim 1–4000. Lần đầu không cần reason. Chọn cùng bộ hiện hành trả 200 no_change, không ghi sự kiện.
- Transaction khóa request, kiểm version, thay cả department+assignee, tăng version, ghi lịch sử old/new public. Một request chỉ có đúng một bộ trách nhiệm hiện hành. Chuyển phòng là thay bộ này, không sao chép hồ sơ.
- Giữ status, due_at, question, result và mọi lịch sử cũ. NV cũ mất quyền trên các request mới ngay sau commit; request đang ghi cạnh tranh bị khóa/version chặn. Đang WaitingInfo thì câu hỏi chuyển theo hồ sơ cho NV mới.
- Resolved/Closed:409. Không tự xóa người để đưa lại hàng chờ, không tự nhận/claim bằng tài khoản không dispatch. RPT-05 chỉ tính bộ hiện hành, không đếm hai phòng.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-04-01:** Khi Giao yêu cầu cho NV-A thuộc PB-A. → Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.
- **AC-DSP-04-02:** Khi Chọn PB-A với nhân viên PB-B. → Từ chối; không tạo trách nhiệm không nhất quán.
- **AC-DSP-04-03:** Khi Nhân viên không có quyền điều phối gửi assignee_id của mình. → Từ chối; không thay trách nhiệm.
- **AC-DSP-04-04:** Khi Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. → Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.
- **AC-DSP-04-05:** Khi Gửi lại cùng giá trị hiện hành với version mới nhất. → 200 no_change; version và số history không tăng.
- **AC-DSP-04-06:** Khi Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. → 422 reason; giá trị/version/history giữ nguyên.
- **AC-DSP-04-07:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-04-08:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-04-08; kết quả thực thi ban đầu là Not Run.
