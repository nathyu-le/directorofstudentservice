### [FR-DSP-09] Ghi kết quả giải quyết

**Mô tả**

Ghi kết quả để sinh viên nhận và phản hồi. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Nhân viên nhập kết quả và xác nhận Đã giải quyết.
2. Máy chủ kiểm tra quyền, trạng thái và câu hỏi bổ sung.
3. Lưu kết quả công khai, resolved_at, chuyển Đã giải quyết và ghi lịch sử cùng giao dịch.
4. Sinh viên thấy kết quả và có thể phản hồi qua STU-05.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| result_content | Bắt buộc | Kết quả/hướng dẫn không rỗng; 4000 ký tự. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ nhân viên được giao, Processing, không có question mở. result_content sau khi bỏ khoảng trắng đầu/cuối, dài 1–4000 bắt buộc. **Kiểm bởi:** AC-DSP-09-01, AC-DSP-09-02, AC-DSP-09-03, AC-DSP-09-07, AC-DSP-09-08.
- Transaction tạo result duy nhất/request, chuyển Processing→Resolved, set resolved_at=server_now, tăng version và history RESOLVED public. Kết quả được STU-03 đọc. **Kiểm bởi:** AC-DSP-09-01, AC-DSP-09-05, AC-DSP-09-08.
- Giữ started_at, due_at và trách nhiệm. Không đồng thời đóng request, không tự ghi feedback. Sau Resolved không có sửa/xóa kết quả trong baseline. **Kiểm bởi:** AC-DSP-09-01, AC-DSP-09-04, AC-DSP-09-08.
- WaitingInfo/Closed/Resolved hoặc version cũ trả 409. Hai lần giải quyết không tạo hai result; lỗi ghi history rollback cả result và status. **Kiểm bởi:** AC-DSP-09-04, AC-DSP-09-05, AC-DSP-09-06, AC-DSP-09-07, AC-DSP-09-08.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền xử lý hoặc CSRF. **Kiểm bởi:** NFR-SEC-01.
- 404 hồ sơ không giao cho actor. **Kiểm bởi:** AC-DSP-09-03.
- 422 thiếu kết quả hoặc nội dung quá dài. **Kiểm bởi:** AC-DSP-09-02, AC-DSP-09-08.
- 409 còn chờ, đã kết thúc hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-09-04, AC-DSP-09-06, AC-DSP-09-07.
- 500 hoàn tác kết quả và trạng thái. **Kiểm bởi:** AC-DSP-09-05.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-09-01:** Khi Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận. → Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.
- **AC-DSP-09-02:** Khi Chờ bổ sung hoặc resolution rỗng. → Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.
- **AC-DSP-09-03:** Khi NV-B ghi kết quả cho hồ sơ NV-A. → Từ chối; không thay kết quả.
- **AC-DSP-09-04:** Khi Gửi lại cùng thao tác hoặc dùng record_version cũ. → Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.
- **AC-DSP-09-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-09-06:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.
- **AC-DSP-09-07:** Khi WaitingInfo có question mở; NV gửi result_content. → 409 INVALID_STATE; không result và không resolved_at mới.

- **AC-DSP-09-08:** Khi NV-A giải quyết hồ sơ 315 đang Processing với nội dung 4000 ký tự rồi thử lặp. Trên fixture độc lập thử nội dung 4001 ký tự. → Một kết quả và một RESOLVED công khai. Giữ hạn, giờ bắt đầu, trách nhiệm, không tự đóng hoặc tạo phản hồi. Lặp trả 409, quá dài trả 422.

**Ví dụ Edge Case**

Không giải quyết khi chờ: WaitingInfo có question mở; NV gửi result_content.

**Expected Result**

409 INVALID_STATE; không result và không resolved_at mới. Kiểm theo TC-DSP-09-07; kết quả thực thi ban đầu là Not Run.

