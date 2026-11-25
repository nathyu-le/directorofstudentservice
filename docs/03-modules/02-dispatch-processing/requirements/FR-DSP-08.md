### [FR-DSP-08] Ghi cập nhật tiến độ xử lý

**Mô tả**

Ghi công việc đã thực hiện để phối hợp và theo dõi. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Nhân viên được giao. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P1 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- Chỉ nhân viên được giao, Processing/WaitingInfo. content sau khi bỏ khoảng trắng đầu/cuối, dài 1–4000; visibility enum public/internal, mặc định internal trên UI và máy chủ khi không gửi. **Kiểm bởi:** AC-DSP-08-01, AC-DSP-08-02, AC-DSP-08-03, AC-DSP-08-07.
- Transaction tạo progress_note và history gắn cùng request, tăng version. Không thay status, assignee, due_at hoặc đóng câu hỏi. **Kiểm bởi:** AC-DSP-08-01, AC-DSP-08-04, AC-DSP-08-05, AC-DSP-08-07.
- public được chủ sinh viên đọc; internal chỉ người nghiệp vụ có scope. Máy chủ không trả internal trong STU-03 ngay cả khi client tự đặt include_internal=true. **Kiểm bởi:** AC-DSP-08-01, AC-DSP-08-04, AC-DSP-08-07.
- Hai tab dùng cùng version: chỉ lần lưu đầu thành công, lần sau trả 409. Không chống trùng nội dung giữa hai thao tác khác nhau; hai cập nhật có ý định riêng được lưu thành hai note. **Kiểm bởi:** AC-DSP-08-05, AC-DSP-08-06, AC-DSP-08-07.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền xử lý hoặc CSRF. **Kiểm bởi:** NFR-SEC-01.
- 404 đã mất trách nhiệm. **Kiểm bởi:** AC-DSP-08-03.
- 422 nội dung hoặc phạm vi công khai không hợp lệ. **Kiểm bởi:** AC-DSP-08-02, AC-DSP-08-07.
- 409 trạng thái đóng hoặc phiên bản cũ. **Kiểm bởi:** AC-DSP-08-06, AC-DSP-08-07.
- 500 hoàn tác ghi chú và lịch sử. **Kiểm bởi:** AC-DSP-08-05.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-DSP-08-01:** Khi Ghi Đang kiểm tra thủ tục, visibility=public. → Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.
- **AC-DSP-08-02:** Khi Nội dung trống hoặc visibility=secret. → Từ chối; không thêm lịch sử.
- **AC-DSP-08-03:** Khi NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. → Từ chối; không ghi nội dung mới.
- **AC-DSP-08-04:** Khi Ghi ghi chú internal có dấu TEST-INTERNAL. → Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.
- **AC-DSP-08-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-08-06:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-DSP-08-07:** Khi Trên hồ sơ WaitingInfo 311, NV-A gửi hai ghi chú cùng nội dung bằng hai thao tác với phiên bản mới, lần đầu bỏ visibility. Thử thêm nội dung 4001 ký tự và hồ sơ Closed. → Ghi chú mặc định internal, hai thao tác riêng có hai mục. Giữ nguyên câu hỏi, hạn, trạng thái và người xử lý. Trả 422 cho nội dung quá dài và 409 cho Closed.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-08-06; kết quả thực thi ban đầu là Not Run.

