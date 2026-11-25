### [FR-STU-05] Phản hồi kết quả hỗ trợ

**Mô tả**

Ghi nhận phản hồi và điểm hài lòng về kết quả. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P1 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

**Preconditions**

- Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Sinh viên đọc kết quả, chọn điểm và nhập nhận xét nếu có.
2. Giao diện gửi result_id, record_version cùng score/comment; máy chủ kiểm version và kết quả hiện hành.
3. Máy chủ kiểm tra chủ hồ sơ, kết quả và điểm.
4. Lưu một phản hồi hiện hành cho yêu cầu; gửi lại cập nhật phản hồi đó.
5. Hiển thị phản hồi đã lưu; quản lý đọc qua báo cáo hài lòng. Không tự mở lại yêu cầu.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| score | Bắt buộc | Số nguyên 1–5 theo điểm nguyên 1–5; có nhãn ý nghĩa. |
| comment | Tùy chọn | Nhận xét tối đa 4000 ký tự. |
| result_id | Hệ thống | Kết quả hiện hành của yêu cầu. |
| record_version | Bắt buộc | Số nguyên >=1; kiểm tra phiên bản request khi upsert phản hồi. |

- score nguyên 1–5: 1 rất không hài lòng, 2 không hài lòng, 3 bình thường, 4 hài lòng, 5 rất hài lòng. comment tùy chọn, trim, tối đa 4000. result_id hiện hành và record_version được gửi để đối chiếu. **Kiểm bởi:** AC-STU-05-01, AC-STU-05-02, AC-STU-05-08, AC-STU-05-09.
- Chỉ chủ hồ sơ Resolved/Closed có kết quả được tạo/cập nhật feedback. UNIQUE(feedback.request_id). Một feedback hiện hành/request, không một dòng mới mỗi lần đổi điểm. **Kiểm bởi:** AC-STU-05-01, AC-STU-05-03, AC-STU-05-04, AC-STU-05-05.
- Transaction upsert feedback và history, tăng request.version, giữ trạng thái/owner/kết quả. feedback.created_at giữ lần đầu, updated_at là lần sửa cuối. Không tự đóng/mở lại. **Kiểm bởi:** AC-STU-05-04, AC-STU-05-05, AC-STU-05-06, AC-STU-05-07, AC-STU-05-09.
- RPT-06 lọc theo feedback.updated_at. Sửa feedback có thể đưa nó sang kỳ cập nhật mới; giao diện báo cáo phải ghi rõ tiêu chí này, không gọi đó là khảo sát lịch sử bất biến. **Kiểm bởi:** AC-STU-05-09.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền hành động hoặc CSRF. **Kiểm bởi:** NFR-SEC-01.
- 404 khác chủ hồ sơ. **Kiểm bởi:** AC-STU-05-03.
- 422 điểm/nhận xét sai, kết quả hoặc dữ liệu đầu vào không hợp lệ. **Kiểm bởi:** AC-STU-05-02, AC-STU-05-08.
- 409 trạng thái hoặc phiên bản xung đột. **Kiểm bởi:** AC-STU-05-02, AC-STU-05-07.
- 500 hoàn tác phản hồi. **Kiểm bởi:** AC-STU-05-06.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-STU-05-01:** Khi Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn. → Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.
- **AC-STU-05-02:** Khi Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. → Từ chối; không có phản hồi mới để tính báo cáo.
- **AC-STU-05-03:** Khi SV-B gửi đánh giá cho yêu cầu SV-A. → Từ chối; phản hồi của SV-A không bị sửa.
- **AC-STU-05-04:** Khi SV-A đổi điểm từ 4 thành 3. → Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.
- **AC-STU-05-05:** Khi Chủ SV gửi score 5 trên Closed có result. → Lưu feedback, status vẫn Closed, closed_at không đổi.
- **AC-STU-05-06:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-STU-05-07:** Khi Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ. → Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

- **AC-STU-05-08:** Khi Trên hồ sơ 313 gửi nhận xét có khoảng trắng đầu/cuối với 4000 rồi 4001 ký tự sau bỏ khoảng trắng. Thử result_id của hồ sơ khác và version hiện hành. → Chấp nhận nhận xét hợp lệ, báo trường sai cho nhận xét 4001, từ chối result_id sai. Giữ kết quả, trạng thái và phản hồi cũ ở các lượt lỗi.
- **AC-STU-05-09:** Khi Thêm phản hồi 4 điểm trên hồ sơ 313, đổi thành 3 ở ngày hôm sau. Đọc nguồn của RPT-06 cho hai ngày. → Giữ feedback_id và created_at, cập nhật updated_at, không sinh dòng phản hồi thứ hai. Hồ sơ và kết quả giữ nguyên; báo cáo dùng kỳ cập nhật mới.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tab A lưu hợp lệ, tab B gửi version cũ.

**Expected Result**

Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại. Kiểm theo TC-STU-05-07; kết quả thực thi ban đầu là Not Run.

