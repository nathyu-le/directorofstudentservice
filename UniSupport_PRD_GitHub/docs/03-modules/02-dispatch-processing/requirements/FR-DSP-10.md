# [FR-DSP-10] Đóng yêu cầu đã giải quyết

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Kết thúc hồ sơ đã có kết quả sau kiểm tra; bảo toàn lịch sử.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ đã giải quyết. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên kiểm tra kết quả và chọn Đóng yêu cầu.
2. Máy chủ kiểm tra quyền và trạng thái Đã giải quyết.
3. Chuyển Đã đóng, ghi closed_at và lịch sử; giữ kết quả, trách nhiệm và phản hồi.
4. Ngừng thao tác xử lý; vẫn cho người có quyền xem và sinh viên phản hồi kết quả.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Đóng trước khi giải quyết: Hồ sơ Đang xử lý chưa có kết quả. → Từ chối; không bỏ qua bước giải quyết.
- Sai vai trò: Sinh viên hoặc nhân viên thường gọi thao tác đóng. → Từ chối; trạng thái không đổi.
- Đóng lặp: Gửi lại thao tác đóng; sau đó thử ghi tiến độ. → Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-10-01 | Đóng đúng | Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên. |
| AC-DSP-10-02 | Đóng trước khi giải quyết | Từ chối; không bỏ qua bước giải quyết. |
| AC-DSP-10-03 | Sai vai trò | Từ chối; trạng thái không đổi. |
| AC-DSP-10-04 | Đóng lặp | Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-10-01 | AC-DSP-10-01 | Điều phối viên đóng yêu cầu Đã giải quyết có kết quả. |
| TC-DSP-10-02 | AC-DSP-10-02 | Hồ sơ Đang xử lý chưa có kết quả. |
| TC-DSP-10-03 | AC-DSP-10-03 | Sinh viên hoặc nhân viên thường gọi thao tác đóng. |
| TC-DSP-10-04 | AC-DSP-10-04 | Gửi lại thao tác đóng; sau đó thử ghi tiến độ. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.

Master dùng parent `FR-DSP-10 | Đóng yêu cầu đã giải quyết` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-09
