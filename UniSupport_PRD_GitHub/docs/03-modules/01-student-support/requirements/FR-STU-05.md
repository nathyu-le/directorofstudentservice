# [FR-STU-05] Phản hồi kết quả hỗ trợ

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Ghi nhận phản hồi và điểm hài lòng về kết quả.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| score | Bắt buộc | Số nguyên 1–5 theo CFG-05; có nhãn ý nghĩa. |
| comment | Tùy chọn | Nhận xét tối đa CFG-02. |
| result_id | Hệ thống | Kết quả hiện hành của yêu cầu. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên đọc kết quả, chọn điểm và nhập nhận xét nếu có.
2. Máy chủ kiểm tra chủ hồ sơ, kết quả và điểm.
3. Lưu một phản hồi hiện hành cho yêu cầu; gửi lại cập nhật phản hồi đó.
4. Hiển thị phản hồi đã lưu; quản lý đọc qua báo cáo hài lòng. Không tự mở lại yêu cầu.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Điểm sai / chưa có kết quả: Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. → Từ chối; không có phản hồi mới để tính báo cáo.
- Không phải chủ: SV-B gửi đánh giá cho yêu cầu SV-A. → Từ chối; phản hồi của SV-A không bị sửa.
- Cập nhật phản hồi: SV-A đổi điểm từ 4 thành 3. → Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-05-01 | Phản hồi hợp lệ | Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4. |
| AC-STU-05-02 | Điểm sai / chưa có kết quả | Từ chối; không có phản hồi mới để tính báo cáo. |
| AC-STU-05-03 | Không phải chủ | Từ chối; phản hồi của SV-A không bị sửa. |
| AC-STU-05-04 | Cập nhật phản hồi | Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-05-01 | AC-STU-05-01 | Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn. |
| TC-STU-05-02 | AC-STU-05-02 | Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. |
| TC-STU-05-03 | AC-STU-05-03 | SV-B gửi đánh giá cho yêu cầu SV-A. |
| TC-STU-05-04 | AC-STU-05-04 | SV-A đổi điểm từ 4 thành 3. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.

Master dùng parent `FR-STU-05 | Phản hồi kết quả hỗ trợ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-09
