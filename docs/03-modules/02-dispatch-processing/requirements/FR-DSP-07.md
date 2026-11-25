### [FR-DSP-07] Yêu cầu sinh viên bổ sung thông tin

**Mô tả**

Nêu thông tin còn thiếu và đặt yêu cầu vào trạng thái chờ trả lời. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Nhân viên ghi rõ thông tin còn thiếu và chọn Yêu cầu bổ sung.
2. Máy chủ kiểm tra quyền, trạng thái và không có câu hỏi mở khác.
3. Lưu câu hỏi, chuyển Chờ bổ sung và thêm sự kiện công khai.
4. Sinh viên thấy câu hỏi trong STU-03 và trả lời qua STU-04.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| question | Bắt buộc | Câu hỏi cụ thể không rỗng; 4000 ký tự. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ nhân viên được giao, Processing, chưa có question mở. question sau khi bỏ khoảng trắng đầu/cuối, dài 1–4000 phải nêu thông tin cần bổ sung. **Kiểm bởi:** AC-DSP-07-01, AC-DSP-07-02, AC-DSP-07-03, AC-DSP-07-08.
- Transaction tạo question gắn request, visibility public, chưa có answer; chuyển Processing→WaitingInfo, tăng version và history INFO_REQUESTED. **Kiểm bởi:** AC-DSP-07-01, AC-DSP-07-05, AC-DSP-07-06.
- Một question mở tối đa/request được bảo vệ bởi khóa request khi ghi. Câu hỏi đã trả lời được giữ lịch sử; sau trở lại Processing có thể hỏi đợt mới với question_id khác. **Kiểm bởi:** AC-DSP-07-04, AC-DSP-07-07.
- Không tự đặt lại due_at/started_at. Không gửi mail/notification ngoài baseline. Gửi lặp hoặc WaitingInfo:409; lỗi database rollback cả question và status. **Kiểm bởi:** AC-DSP-07-04, AC-DSP-07-05, AC-DSP-07-08.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền xử lý hoặc CSRF. **Kiểm bởi:** NFR-SEC-01.
- 404 hồ sơ không giao cho actor. **Kiểm bởi:** AC-DSP-07-03.
- 422 câu hỏi không hợp lệ. **Kiểm bởi:** AC-DSP-07-02, AC-DSP-07-08.
- 409 chưa xử lý, đã chờ bổ sung hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-07-04, AC-DSP-07-06, AC-DSP-07-08.
- 500 hoàn tác cả câu hỏi và trạng thái. **Kiểm bởi:** AC-DSP-07-05.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-07-01:** Khi Đang xử lý, hỏi Vui lòng cung cấp mã lớp. → Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.
- **AC-DSP-07-02:** Khi Nội dung chỉ có khoảng trắng. → Không lưu câu hỏi hoặc chuyển trạng thái.
- **AC-DSP-07-03:** Khi NV-B yêu cầu bổ sung hồ sơ giao NV-A. → Từ chối; không lộ thêm dữ liệu.
- **AC-DSP-07-04:** Khi Gửi lần thứ hai khi Chờ bổ sung. → HTTP 409 INVALID_STATE; vẫn một câu hỏi mở, không có history/question thứ hai.
- **AC-DSP-07-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-07-06:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.
- **AC-DSP-07-07:** Khi Sau STU-04 đóng câu hỏi 1 và Processing, NV hỏi câu hỏi 2. → Có câu hỏi 2 ID khác, một câu hỏi mở; câu hỏi 1/answer cũ giữ nguyên.

- **AC-DSP-07-08:** Khi NV-A thử tạo câu hỏi trên hồ sơ Received đã được giao, sau đó chuyển sang Processing và thử câu hỏi 4001 ký tự. → Received trả 409, văn bản quá dài trả 422. Không đổi trạng thái/hạn/thời điểm bắt đầu, không sinh question/history.

**Ví dụ Edge Case**

Đợt bổ sung tiếp theo: Sau STU-04 đóng câu hỏi 1 và Processing, NV hỏi câu hỏi 2.

**Expected Result**

Có câu hỏi 2 ID khác, một câu hỏi mở; câu hỏi 1/answer cũ giữ nguyên. Kiểm theo TC-DSP-07-07; kết quả thực thi ban đầu là Not Run.

