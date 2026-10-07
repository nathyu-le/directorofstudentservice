# [FR-DSP-06] Bắt đầu xử lý yêu cầu

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Xác nhận bắt đầu xử lý yêu cầu đã tiếp nhận.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ được giao cho người thao tác. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên mở yêu cầu được giao và chọn Bắt đầu xử lý.
2. Máy chủ kiểm tra trách nhiệm và trạng thái.
3. Chuyển Đã tiếp nhận → Đang xử lý, ghi thời điểm bắt đầu và lịch sử công khai.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Trạng thái sai: Bắt đầu hồ sơ Đã đóng. → Từ chối; không thay trạng thái và thời điểm.
- Người khác thao tác: NV-B bắt đầu hồ sơ giao NV-A. → Từ chối; trách nhiệm giữ nguyên.
- Bấm hai lần: Gửi lặp lần bắt đầu. → Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-06-01 | Bắt đầu đúng | Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ. |
| AC-DSP-06-02 | Trạng thái sai | Từ chối; không thay trạng thái và thời điểm. |
| AC-DSP-06-03 | Người khác thao tác | Từ chối; trách nhiệm giữ nguyên. |
| AC-DSP-06-04 | Bấm hai lần | Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-06-01 | AC-DSP-06-01 | NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình. |
| TC-DSP-06-02 | AC-DSP-06-02 | Bắt đầu hồ sơ Đã đóng. |
| TC-DSP-06-03 | AC-DSP-06-03 | NV-B bắt đầu hồ sơ giao NV-A. |
| TC-DSP-06-04 | AC-DSP-06-04 | Gửi lặp lần bắt đầu. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.

Master dùng parent `FR-DSP-06 | Bắt đầu xử lý yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-04
