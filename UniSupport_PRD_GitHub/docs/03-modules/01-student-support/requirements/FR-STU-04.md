# [FR-STU-04] Bổ sung thông tin được yêu cầu

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Trả lời yêu cầu bổ sung trên cùng hồ sơ hỗ trợ.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| question_id | Bắt buộc | Câu hỏi đang mở thuộc yêu cầu. |
| content | Bắt buộc | Nội dung không rỗng; CFG-02. |
| record_version | Hệ thống | Phiên bản dùng khi gửi để phát hiện dữ liệu đã đổi. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên đọc câu hỏi và nhập nội dung trả lời.
2. Máy chủ kiểm tra chủ hồ sơ, trạng thái, câu hỏi và phiên bản.
3. Lưu câu trả lời vào cùng request_id, đánh dấu câu hỏi đã được trả lời và chuyển về Đang xử lý.
4. Giữ người phụ trách; thêm lịch sử công khai để bên xử lý tiếp tục.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Nội dung trống: Trả lời chỉ có khoảng trắng. → Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.
- Sai chủ: SV-B gửi câu trả lời cho yêu cầu SV-A. → Từ chối; nội dung và trạng thái không đổi.
- Gửi từ màn hình cũ: Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. → Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-04-01 | Trả lời hợp lệ | Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý. |
| AC-STU-04-02 | Nội dung trống | Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời. |
| AC-STU-04-03 | Sai chủ | Từ chối; nội dung và trạng thái không đổi. |
| AC-STU-04-04 | Gửi từ màn hình cũ | Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-04-01 | AC-STU-04-01 | Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01. |
| TC-STU-04-02 | AC-STU-04-02 | Trả lời chỉ có khoảng trắng. |
| TC-STU-04-03 | AC-STU-04-03 | SV-B gửi câu trả lời cho yêu cầu SV-A. |
| TC-STU-04-04 | AC-STU-04-04 | Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.

Master dùng parent `FR-STU-04 | Bổ sung thông tin được yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-07, FR-STU-03
