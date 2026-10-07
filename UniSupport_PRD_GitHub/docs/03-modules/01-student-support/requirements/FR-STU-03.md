# [FR-STU-03] Xem chi tiết và tiến độ yêu cầu

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Biết yêu cầu đã được tiếp nhận, ai chịu trách nhiệm và có cần bổ sung thông tin.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Mã yêu cầu thuộc người dùng. |
| status / owner / due_at | Chỉ đọc | Lấy từ bản ghi hiện hành; hạn có thể chưa xác định. |
| public_history | Chỉ đọc | Các sự kiện và nội dung được công khai cho sinh viên. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên mở mã yêu cầu.
2. Máy chủ kiểm tra quyền trên yêu cầu trước khi đọc nội dung.
3. Hiển thị nội dung, trạng thái, trách nhiệm, hạn nếu có và lịch sử công khai.
4. Nếu Chờ bổ sung, hiển thị câu hỏi đang mở và thao tác bổ sung; nếu có kết quả, hiển thị kết quả và phản hồi.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Mã không tồn tại: Mã yêu cầu không có trong bộ dữ liệu. → Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.
- Truy cập ngang: SV-B mở URL yêu cầu của SV-A. → Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A.
- Chưa phân công / chờ bổ sung: Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. → Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-03-01 | Tiến độ đã cập nhật | Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại. |
| AC-STU-03-02 | Mã không tồn tại | Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu. |
| AC-STU-03-03 | Truy cập ngang | Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A. |
| AC-STU-03-04 | Chưa phân công / chờ bổ sung | Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-03-01 | AC-STU-03-01 | Yêu cầu được phân công rồi chuyển Đang xử lý. |
| TC-STU-03-02 | AC-STU-03-02 | Mã yêu cầu không có trong bộ dữ liệu. |
| TC-STU-03-03 | AC-STU-03-03 | SV-B mở URL yêu cầu của SV-A. |
| TC-STU-03-04 | AC-STU-03-04 | Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.

Master dùng parent `FR-STU-03 | Xem chi tiết và tiến độ yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-IAM-03
