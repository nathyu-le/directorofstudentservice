# [FR-DSP-02] Xem hồ sơ xử lý và lịch sử nghiệp vụ

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Đọc thông tin cần xử lý và truy vết thao tác theo trách nhiệm.

## Actor

Điều phối viên / Nhân viên xử lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền trên yêu cầu theo vai trò và phạm vi công việc.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Yêu cầu thuộc phạm vi được xem. |
| history | Chỉ đọc | Người, thời gian, hành động và thay đổi; phân biệt nội bộ/công khai. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Người dùng mở hồ sơ từ hàng chờ hoặc danh sách được giao.
2. Máy chủ kiểm tra vai trò và phạm vi yêu cầu.
3. Hiển thị thông tin sinh viên tối thiểu phục vụ xử lý, nội dung yêu cầu, trách nhiệm và lịch sử có thứ tự.
4. Giao diện chỉ đưa thao tác phù hợp trạng thái và quyền; máy chủ vẫn kiểm tra khi thực hiện.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Mã không có: Mở mã không tồn tại. → Không tìm thấy; không sinh dữ liệu.
- Ngoài phạm vi: Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. → Từ chối, không trả nội dung sinh viên.
- Ghi chú nội bộ: Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. → Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-02-01 | Có lịch sử | Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi. |
| AC-DSP-02-02 | Mã không có | Không tìm thấy; không sinh dữ liệu. |
| AC-DSP-02-03 | Ngoài phạm vi | Từ chối, không trả nội dung sinh viên. |
| AC-DSP-02-04 | Ghi chú nội bộ | Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-02-01 | AC-DSP-02-01 | Yêu cầu đã tiếp nhận, phân loại, phân công. |
| TC-DSP-02-02 | AC-DSP-02-02 | Mở mã không tồn tại. |
| TC-DSP-02-03 | AC-DSP-02-03 | Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. |
| TC-DSP-02-04 | AC-DSP-02-04 | Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.

Master dùng parent `FR-DSP-02 | Xem hồ sơ xử lý và lịch sử nghiệp vụ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-IAM-03
