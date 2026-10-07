# [FR-DSP-04] Phân công trách nhiệm xử lý

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Đặt một đơn vị và một người chịu trách nhiệm hiện tại, kể cả khi phối hợp đổi đơn vị.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| department_id | Bắt buộc | Đơn vị mẫu đang dùng. |
| assignee_id | Bắt buộc | Nhân viên hoạt động thuộc đơn vị. |
| reason | Khi đổi | Lý do đổi trách nhiệm không rỗng. |
| record_version | Hệ thống | Khóa kiểm soát cập nhật đồng thời. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên chọn đơn vị và nhân viên phụ trách.
2. Nếu đổi đơn vị/người, nhập lý do phối hợp và xác nhận.
3. Máy chủ kiểm tra quyền, quan hệ người–đơn vị và phiên bản.
4. Thay bộ trách nhiệm trong một giao dịch, giữ trạng thái và ghi người/đơn vị cũ–mới; quyền xử lý chuyển sang người mới.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Người không thuộc đơn vị: Chọn PB-A với nhân viên PB-B. → Từ chối; không tạo trách nhiệm không nhất quán.
- Tự chiếm yêu cầu: Nhân viên không có quyền điều phối gửi assignee_id của mình. → Từ chối; không thay trách nhiệm.
- Đổi trách nhiệm đồng thời: Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. → Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-04-01 | Phân công lần đầu | Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới. |
| AC-DSP-04-02 | Người không thuộc đơn vị | Từ chối; không tạo trách nhiệm không nhất quán. |
| AC-DSP-04-03 | Tự chiếm yêu cầu | Từ chối; không thay trách nhiệm. |
| AC-DSP-04-04 | Đổi trách nhiệm đồng thời | Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-04-01 | AC-DSP-04-01 | Giao yêu cầu cho NV-A thuộc PB-A. |
| TC-DSP-04-02 | AC-DSP-04-02 | Chọn PB-A với nhân viên PB-B. |
| TC-DSP-04-03 | AC-DSP-04-03 | Nhân viên không có quyền điều phối gửi assignee_id của mình. |
| TC-DSP-04-04 | AC-DSP-04-04 | Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.

Master dùng parent `FR-DSP-04 | Phân công trách nhiệm xử lý` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-03, FR-IAM-03
