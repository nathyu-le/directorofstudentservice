### [FR-DSP-10] Đóng yêu cầu đã giải quyết

**Mô tả**

Kết thúc hồ sơ đã có kết quả sau kiểm tra; bảo toàn lịch sử. Đặc tả triển khai prototype thuộc Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Điều phối viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Điều phối viên kiểm tra kết quả và chọn Đóng yêu cầu.
2. Máy chủ kiểm tra quyền và trạng thái Đã giải quyết.
3. Chuyển Đã đóng, ghi closed_at và lịch sử; giữ kết quả, trách nhiệm và phản hồi.
4. Ngừng thao tác xử lý; vẫn cho người có quyền xem và sinh viên phản hồi kết quả.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ đã giải quyết. |
| record_version | Hệ thống | Phiên bản hiện hành. |

- Chỉ dispatch trong scope, status=Resolved và result tồn tại. record_version bắt buộc. Đóng là xác nhận kết thúc nghiệp vụ, không đợi một điểm hài lòng bắt buộc.
- Transaction Resolved→Closed, set closed_at=server_now, tăng version và history CLOSED public. Không xóa kết quả hoặc dữ liệu.
- Closed chỉ đọc và STU-05 phản hồi/cập nhật điểm. Không phân loại/phân công/đặt hạn/bổ sung/tiến độ/giải quyết lại. Không tự đóng sau thời gian cố định.
- Gửi lặp/trạng thái trước Resolved/version cũ:409. Nhân viên xử lý không dispatch:403. Đây không là chức năng mở lại/hủy.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 409: trạng thái/phiên bản/khóa xung đột theo quy tắc của chức năng; không ghi một phần, yêu cầu tải lại dữ liệu hiện hành.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-DSP-10-01:** Khi Điều phối viên đóng yêu cầu Đã giải quyết có kết quả. → Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.
- **AC-DSP-10-02:** Khi Hồ sơ Đang xử lý chưa có kết quả. → Từ chối; không bỏ qua bước giải quyết.
- **AC-DSP-10-03:** Khi Sinh viên hoặc nhân viên thường gọi thao tác đóng. → Từ chối; trạng thái không đổi.
- **AC-DSP-10-04:** Khi Gửi lại thao tác đóng; sau đó thử ghi tiến độ. → Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.
- **AC-DSP-10-05:** Khi Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production. → 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.
- **AC-DSP-10-06:** Khi Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ. → TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

**Ví dụ Edge Case**

Cập nhật phiên bản cũ: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

**Expected Result**

TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại. Kiểm theo TC-DSP-10-06; kết quả thực thi ban đầu là Not Run.
