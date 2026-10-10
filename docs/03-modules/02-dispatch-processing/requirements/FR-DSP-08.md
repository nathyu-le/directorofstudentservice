### [FR-DSP-08] Ghi cập nhật tiến độ xử lý

**Mô tả**

Ghi công việc đã thực hiện để phối hợp và theo dõi. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Nhân viên nhập tiến độ và chọn công khai hoặc nội bộ.
2. Máy chủ kiểm tra quyền, trạng thái và nội dung.
3. Thêm sự kiện tiến độ có người ghi, thời gian và mức hiển thị.
4. Giữ trạng thái/trách nhiệm; chỉ nội dung công khai hiện cho sinh viên.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| content | Bắt buộc | Không rỗng; 4000 ký tự. |
| visibility | Tùy chọn, mặc định internal | public hoặc internal; mặc định internal. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ nhân viên được giao, Processing/WaitingInfo. content trim1–4000; visibility enum public/internal, mặc định internal trên UI và máy chủ khi không gửi.
- Transaction tạo progress_note và history gắn cùng request, tăng version. Không thay status, assignee, due_at hoặc đóng câu hỏi.
- public được chủ sinh viên đọc; internal chỉ người nghiệp vụ có scope. Máy chủ không trả internal trong STU-03 ngay cả khi client tự đặt include_internal=true.
- Hai tab dùng cùng version: chỉ lần lưu đầu thành công, lần sau409. Không chống trùng nội dung giữa hai thao tác khác nhau; hai cập nhật có ý định riêng được lưu thành hai note.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-08-01:** Khi Ghi Đang kiểm tra thủ tục, visibility=public. → Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.
- **AC-DSP-08-02:** Khi Nội dung trống hoặc visibility=secret. → Từ chối; không thêm lịch sử.
- **AC-DSP-08-03:** Khi NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. → Từ chối; không ghi nội dung mới.
- **AC-DSP-08-04:** Khi Ghi ghi chú internal có dấu TEST-INTERNAL. → Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.
- **AC-DSP-08-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-08-06:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-08-06; kết quả thực thi ban đầu là Not Run.
