# [FR-DSP-09] Ghi kết quả giải quyết

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Ghi kết quả để sinh viên nhận và phản hồi.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| resolution | Bắt buộc | Kết quả/hướng dẫn không rỗng; CFG-02. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên nhập kết quả và xác nhận Đã giải quyết.
2. Máy chủ kiểm tra quyền, trạng thái và câu hỏi bổ sung.
3. Lưu kết quả công khai, resolved_at, chuyển Đã giải quyết và ghi lịch sử cùng giao dịch.
4. Sinh viên thấy kết quả và có thể phản hồi qua STU-05.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Còn thiếu thông tin / kết quả trống: Chờ bổ sung hoặc resolution rỗng. → Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.
- Sai người xử lý: NV-B ghi kết quả cho hồ sơ NV-A. → Từ chối; không thay kết quả.
- Gửi lại / dữ liệu cũ: Gửi lại cùng thao tác hoặc dùng record_version cũ. → Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-09-01 | Có kết quả | Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung. |
| AC-DSP-09-02 | Còn thiếu thông tin / kết quả trống | Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần. |
| AC-DSP-09-03 | Sai người xử lý | Từ chối; không thay kết quả. |
| AC-DSP-09-04 | Gửi lại / dữ liệu cũ | Không tạo hai kết quả; bản cũ không ghi đè kết quả mới. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-09-01 | AC-DSP-09-01 | Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận. |
| TC-DSP-09-02 | AC-DSP-09-02 | Chờ bổ sung hoặc resolution rỗng. |
| TC-DSP-09-03 | AC-DSP-09-03 | NV-B ghi kết quả cho hồ sơ NV-A. |
| TC-DSP-09-04 | AC-DSP-09-04 | Gửi lại cùng thao tác hoặc dùng record_version cũ. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.

Master dùng parent `FR-DSP-09 | Ghi kết quả giải quyết` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-06, FR-DSP-07
