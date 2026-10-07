# [FR-DSP-03] Phân loại yêu cầu

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Gán loại vấn đề để định tuyến và tổng hợp báo cáo.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| category_id | Bắt buộc | Một loại đang dùng; không nhận ID giả. |
| reason | Khi đổi loại | Lý do không rỗng. |
| record_version | Hệ thống | Kiểm tra phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên mở hồ sơ và chọn loại vấn đề.
2. Máy chủ kiểm tra danh mục, quyền và phiên bản.
3. Lưu loại, lý do nếu đổi, lịch sử loại cũ/mới.
4. Không tự thay người hoặc phòng ban đang phụ trách; phân công qua DSP-04.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Danh mục sai: Gửi loại đã ngừng dùng hoặc ID không có. → Từ chối; loại và lịch sử không thay đổi.
- Không có quyền: Nhân viên thường hoặc sinh viên tự đổi loại. → Từ chối; loại hiện hành giữ nguyên.
- Đã phân công / thao tác đồng thời: Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. → Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-03-01 | Gán loại | Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này. |
| AC-DSP-03-02 | Danh mục sai | Từ chối; loại và lịch sử không thay đổi. |
| AC-DSP-03-03 | Không có quyền | Từ chối; loại hiện hành giữ nguyên. |
| AC-DSP-03-04 | Đã phân công / thao tác đồng thời | Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-03-01 | AC-DSP-03-01 | Yêu cầu Chưa xác định được gán Học vụ. |
| TC-DSP-03-02 | AC-DSP-03-02 | Gửi loại đã ngừng dùng hoặc ID không có. |
| TC-DSP-03-03 | AC-DSP-03-03 | Nhân viên thường hoặc sinh viên tự đổi loại. |
| TC-DSP-03-04 | AC-DSP-03-04 | Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.

Master dùng parent `FR-DSP-03 | Phân loại yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-02
