# [FR-DSP-01] Xem hàng chờ điều phối

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Nhận biết yêu cầu cần phân loại hoặc phân công.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền điều phối trong phạm vi được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| category / status / department | Tùy chọn | Giá trị danh mục và trạng thái hợp lệ. |
| unassigned | Tùy chọn | Lọc chưa có người phụ trách. |
| page | Tùy chọn | Theo CFG-04. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên mở hàng chờ và chọn bộ lọc.
2. Máy chủ giới hạn phạm vi điều phối rồi lọc yêu cầu.
3. Hiển thị mã, loại, trạng thái, đơn vị, người phụ trách và hạn; cho mở chi tiết nghiệp vụ.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Lọc sai: category_id không tồn tại. → Báo tham số sai; không áp dụng loại khác âm thầm.
- Vai trò không được phép: Sinh viên gọi trang/điểm truy cập hàng chờ. → Từ chối; không lộ danh sách toàn trường.
- Sau phân công: Phân công yêu cầu rồi lọc unassigned=true. → Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-01-01 | Yêu cầu mới | Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã. |
| AC-DSP-01-02 | Lọc sai | Báo tham số sai; không áp dụng loại khác âm thầm. |
| AC-DSP-01-03 | Vai trò không được phép | Từ chối; không lộ danh sách toàn trường. |
| AC-DSP-01-04 | Sau phân công | Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-01-01 | AC-DSP-01-01 | Tạo yêu cầu Chưa xác định bằng STU-01. |
| TC-DSP-01-02 | AC-DSP-01-02 | category_id không tồn tại. |
| TC-DSP-01-03 | AC-DSP-01-03 | Sinh viên gọi trang/điểm truy cập hàng chờ. |
| TC-DSP-01-04 | AC-DSP-01-04 | Phân công yêu cầu rồi lọc unassigned=true. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.

Master dùng parent `FR-DSP-01 | Xem hàng chờ điều phối` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-IAM-03
