# [FR-DSP-05] Đặt hạn xử lý dự kiến

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Cung cấp mốc theo dõi quá hạn cho yêu cầu; không tự áp SLA thực tế.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| due_at | Bắt buộc khi đặt hạn | Ngày giờ không trước created_at; cấu hình múi giờ CFG-06. |
| reason | Khi đổi hạn | Lý do không rỗng. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên chọn hạn dự kiến; nếu đổi hạn thì ghi lý do.
2. Kiểm tra hạn, quyền và phiên bản.
3. Lưu hạn và lịch sử thay đổi, không đổi trạng thái hoặc người phụ trách.
4. Chi tiết và báo cáo quá hạn sử dụng hạn hiện hành.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Hạn trước khi tạo: due_at nhỏ hơn created_at. → Từ chối; hạn cũ giữ nguyên.
- Sai quyền: Sinh viên hoặc nhân viên không điều phối sửa hạn. → Từ chối; không thay hạn.
- Không hạn / đúng ranh giới: Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. → Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-05-01 | Hạn hợp lệ | Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái. |
| AC-DSP-05-02 | Hạn trước khi tạo | Từ chối; hạn cũ giữ nguyên. |
| AC-DSP-05-03 | Sai quyền | Từ chối; không thay hạn. |
| AC-DSP-05-04 | Không hạn / đúng ranh giới | Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-05-01 | AC-DSP-05-01 | Hạn hai ngày sau created_at. |
| TC-DSP-05-02 | AC-DSP-05-02 | due_at nhỏ hơn created_at. |
| TC-DSP-05-03 | AC-DSP-05-03 | Sinh viên hoặc nhân viên không điều phối sửa hạn. |
| TC-DSP-05-04 | AC-DSP-05-04 | Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.

Master dùng parent `FR-DSP-05 | Đặt hạn xử lý dự kiến` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-04
