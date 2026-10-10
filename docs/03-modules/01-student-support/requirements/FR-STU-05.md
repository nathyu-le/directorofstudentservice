### [FR-STU-05] Phản hồi kết quả hỗ trợ

**Mô tả**

Ghi nhận phản hồi và điểm hài lòng về kết quả. Đặc tả triển khai prototype thuộc Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

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

- score nguyên 1–5: 1 rất không hài lòng,2 không hài lòng,3 bình thường,4 hài lòng,5 rất hài lòng. comment tùy chọn, trim, tối đa4000. result_id hiện hành và record_version được gửi để đối chiếu.
- Chỉ chủ hồ sơ Resolved/Closed có kết quả được tạo/cập nhật feedback. UNIQUE(feedback.request_id). Một feedback hiện hành/request, không một dòng mới mỗi lần đổi điểm.
- Transaction upsert feedback và history, tăng request.version, giữ trạng thái/owner/kết quả. feedback.created_at giữ lần đầu, updated_at là lần sửa cuối. Không tự đóng/mở lại.
- RPT-06 lọc theo feedback.updated_at. Sửa feedback có thể đưa nó sang kỳ cập nhật mới; giao diện báo cáo phải ghi rõ tiêu chí này, không gọi đó là khảo sát lịch sử bất biến.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-STU-05-01:** Khi Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn. → Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.
- **AC-STU-05-02:** Khi Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. → Từ chối; không có phản hồi mới để tính báo cáo.
- **AC-STU-05-03:** Khi SV-B gửi đánh giá cho yêu cầu SV-A. → Từ chối; phản hồi của SV-A không bị sửa.
- **AC-STU-05-04:** Khi SV-A đổi điểm từ 4 thành 3. → Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.
- **AC-STU-05-05:** Khi Chủ SV gửi score 5 trên Closed có result. → Lưu feedback, status vẫn Closed, closed_at không đổi.
- **AC-STU-05-06:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-STU-05-07:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-STU-05-07; kết quả thực thi ban đầu là Not Run.
