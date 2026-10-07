# [FR-STU-02] Xem danh sách yêu cầu của tôi

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Tìm yêu cầu mình đã gửi và biết trạng thái hiện tại.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| keyword | Tùy chọn | Tìm mã hoặc tiêu đề; CFG-03. |
| status | Tùy chọn | Một trạng thái hợp lệ hoặc Tất cả. |
| page | Tùy chọn | Số nguyên dương; kích thước trang CFG-04. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên mở Yêu cầu của tôi.
2. Máy chủ giới hạn theo user_id của phiên rồi áp dụng tìm kiếm, lọc và phân trang.
3. Trả mã, tiêu đề, trạng thái, đơn vị/người phụ trách nếu đã có, thời điểm cập nhật; cho mở chi tiết.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Tham số sai: page=0 hoặc trạng thái không thuộc danh sách. → Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.
- Lọc theo người khác: SV-A sửa tham số student_id thành SV-B. → Không trả bất kỳ yêu cầu của SV-B.
- Danh sách rỗng: Tài khoản SV-C chưa có yêu cầu. → Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-02-01 | Có yêu cầu và bộ lọc | Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện. |
| AC-STU-02-02 | Tham số sai | Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền. |
| AC-STU-02-03 | Lọc theo người khác | Không trả bất kỳ yêu cầu của SV-B. |
| AC-STU-02-04 | Danh sách rỗng | Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-02-01 | AC-STU-02-01 | SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý. |
| TC-STU-02-02 | AC-STU-02-02 | page=0 hoặc trạng thái không thuộc danh sách. |
| TC-STU-02-03 | AC-STU-02-03 | SV-A sửa tham số student_id thành SV-B. |
| TC-STU-02-04 | AC-STU-02-04 | Tài khoản SV-C chưa có yêu cầu. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.

Master dùng parent `FR-STU-02 | Xem danh sách yêu cầu của tôi` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01
