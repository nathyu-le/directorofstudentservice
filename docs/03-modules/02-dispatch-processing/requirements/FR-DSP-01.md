### [FR-DSP-01] Xem hàng chờ điều phối

**Mô tả**

Nhận biết yêu cầu cần phân loại hoặc phân công. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

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

- Điều phối là quyền dispatch của nhân viên/quản lý, không là nhóm người dùng thứ tư trong proposal. Scope lấy từ quyền máy chủ. Điều phối toàn trường mới thấy yêu cầu chưa phân công.
- Bộ lọc category_id, department_id nguyên dương trong danh mục và phạm vi; status enum5; unassigned boolean; page>=1,20 dòng/trang. Mặc định còn mở, mọi loại, cả chưa phân công.
- Tìm reference/title chuỗi con, keyword<=100. Sắp created_at tăng dần, id tăng dần để hồ sơ cũ không bị chìm. Tổng và danh sách cùng điều kiện.
- unassigned=true khi assignee_id IS NULL. Sau DSP-04 thành công hồ sơ ra khỏi tập unassigned; vẫn thuộc Tất cả nếu đúng scope. Hàng chờ không tự thay trách nhiệm/trạng thái.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-01-01:** Khi Tạo yêu cầu Chưa xác định bằng STU-01. → Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.
- **AC-DSP-01-02:** Khi category_id không tồn tại. → Báo tham số sai; không áp dụng loại khác âm thầm.
- **AC-DSP-01-03:** Khi Sinh viên gọi trang/điểm truy cập hàng chờ. → Từ chối; không lộ danh sách toàn trường.
- **AC-DSP-01-04:** Khi Phân công yêu cầu rồi lọc unassigned=true. → Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.

**Ví dụ Edge Case**

Sau phân công: Phân công yêu cầu rồi lọc unassigned=true.

**Expected Result**

Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi. Kiểm theo TC-DSP-01-04; kết quả thực thi ban đầu là Not Run.
