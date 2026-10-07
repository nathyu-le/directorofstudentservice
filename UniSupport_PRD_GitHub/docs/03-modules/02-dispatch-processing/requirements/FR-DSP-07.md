# [FR-DSP-07] Yêu cầu sinh viên bổ sung thông tin

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Nêu thông tin còn thiếu và đặt yêu cầu vào trạng thái chờ trả lời.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| question | Bắt buộc | Câu hỏi cụ thể không rỗng; CFG-02. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên ghi rõ thông tin còn thiếu và chọn Yêu cầu bổ sung.
2. Máy chủ kiểm tra quyền, trạng thái và không có câu hỏi mở khác.
3. Lưu câu hỏi, chuyển Chờ bổ sung và thêm sự kiện công khai.
4. Sinh viên thấy câu hỏi trong STU-03 và trả lời qua STU-04.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Câu hỏi rỗng: Nội dung chỉ có khoảng trắng. → Không lưu câu hỏi hoặc chuyển trạng thái.
- Ngoài trách nhiệm: NV-B yêu cầu bổ sung hồ sơ giao NV-A. → Từ chối; không lộ thêm dữ liệu.
- Đã có câu hỏi mở: Gửi lần thứ hai khi Chờ bổ sung. → Từ chối/nhắc câu hỏi hiện hành; không có hai câu hỏi mở.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-07-01 | Câu hỏi hợp lệ | Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi. |
| AC-DSP-07-02 | Câu hỏi rỗng | Không lưu câu hỏi hoặc chuyển trạng thái. |
| AC-DSP-07-03 | Ngoài trách nhiệm | Từ chối; không lộ thêm dữ liệu. |
| AC-DSP-07-04 | Đã có câu hỏi mở | Từ chối/nhắc câu hỏi hiện hành; không có hai câu hỏi mở. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-07-01 | AC-DSP-07-01 | Đang xử lý, hỏi Vui lòng cung cấp mã lớp. |
| TC-DSP-07-02 | AC-DSP-07-02 | Nội dung chỉ có khoảng trắng. |
| TC-DSP-07-03 | AC-DSP-07-03 | NV-B yêu cầu bổ sung hồ sơ giao NV-A. |
| TC-DSP-07-04 | AC-DSP-07-04 | Gửi lần thứ hai khi Chờ bổ sung. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.

Master dùng parent `FR-DSP-07 | Yêu cầu sinh viên bổ sung thông tin` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-06
