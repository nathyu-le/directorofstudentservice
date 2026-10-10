### [FR-DSP-05] Đặt hạn xử lý dự kiến

**Mô tả**

Cung cấp mốc theo dõi quá hạn cho yêu cầu; không tự áp SLA thực tế. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

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

- Chỉ dispatch và Received/Processing/WaitingInfo. due_at bắt buộc, ISO8601 có timezone. So sánh UTC với created_at; cho đặt bằng created_at hoặc thời điểm quá khứ nếu không trước created_at để thể hiện backlog.
- Đổi hạn đã có cần reason trim1–4000. Cùng hạn trả 200 no_change. Transaction cập nhật due_at+version+history; không đổi owner/status.
- Đây là hạn dự kiến do điều phối nhập, không SLA tự động. Prototype không có lịch ngày nghỉ nghiệp vụ, thời gian tạm dừng hoặc thông báo tự gửi.
- Không hạn không tính quá hạn; now=due_at chưa quá hạn; now>due_at và trạng thái còn mở mới quá hạn. Resolved/Closed không xuất hiện trong RPT-02 dù resolved trễ.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-05-01:** Khi Hạn hai ngày sau created_at. → Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.
- **AC-DSP-05-02:** Khi due_at nhỏ hơn created_at. → Từ chối; hạn cũ giữ nguyên.
- **AC-DSP-05-03:** Khi Sinh viên hoặc nhân viên không điều phối sửa hạn. → Từ chối; không thay hạn.
- **AC-DSP-05-04:** Khi Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. → Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.
- **AC-DSP-05-05:** Khi Gửi lại cùng giá trị hiện hành với version mới nhất. → 200 no_change; version và số history không tăng.
- **AC-DSP-05-06:** Khi Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. → 422 reason; giá trị/version/history giữ nguyên.
- **AC-DSP-05-07:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-05-08:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-05-08; kết quả thực thi ban đầu là Not Run.
