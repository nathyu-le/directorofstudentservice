# [FR-STU-01] Gửi yêu cầu hỗ trợ

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Tạo yêu cầu có mã để trường tiếp nhận và sinh viên theo dõi.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| category_id | Bắt buộc | Loại vấn đề đang dùng; cho phép giá trị Chưa xác định để điều phối phân loại. |
| title | Bắt buộc | Văn bản không chỉ có khoảng trắng; tối đa CFG-01. |
| description | Bắt buộc | Nội dung vấn đề không rỗng; tối đa CFG-02. |
| submission_key | Hệ thống | Mã của lần gửi để nhận biết gửi lặp cùng thao tác. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên mở biểu mẫu và chọn loại vấn đề, nhập tiêu đề cùng mô tả.
2. Máy chủ lấy chủ yêu cầu từ phiên, kiểm tra trường và mã lần gửi.
3. Lưu yêu cầu, mã duy nhất, thời điểm tạo, trạng thái Đã tiếp nhận và sự kiện tiếp nhận trong cùng giao dịch.
4. Trả mã xác nhận và liên kết xem tiến độ; yêu cầu xuất hiện trong hàng chờ điều phối.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Thiếu nội dung: Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. → Báo tại trường; không tạo yêu cầu hay lịch sử một phần.
- Giả mạo chủ yêu cầu: Phiên SV-A gửi thêm student_id=SV-B. → Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.
- Gửi lặp: Gửi lại cùng submission_key hai lần. → Trả cùng mã; tổng số yêu cầu tăng một, không hai.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-01-01 | Đủ trường, loại hợp lệ | Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã. |
| AC-STU-01-02 | Thiếu nội dung | Báo tại trường; không tạo yêu cầu hay lịch sử một phần. |
| AC-STU-01-03 | Giả mạo chủ yêu cầu | Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt. |
| AC-STU-01-04 | Gửi lặp | Trả cùng mã; tổng số yêu cầu tăng một, không hai. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-01-01 | AC-STU-01-01 | Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục. |
| TC-STU-01-02 | AC-STU-01-02 | Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. |
| TC-STU-01-03 | AC-STU-01-03 | Phiên SV-A gửi thêm student_id=SV-B. |
| TC-STU-01-04 | AC-STU-01-04 | Gửi lại cùng submission_key hai lần. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.

Master dùng parent `FR-STU-01 | Gửi yêu cầu hỗ trợ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-IAM-03
