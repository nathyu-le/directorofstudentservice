# [FR-RPT-06] Tổng hợp mức hài lòng

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Tổng hợp phản hồi kết quả để đánh giá chất lượng hỗ trợ.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| score / feedback | Nguồn đọc | Một phản hồi hiện hành mỗi yêu cầu; điểm trung bình = tổng điểm / số phản hồi; kèm phân bố 1–5 và số phản hồi. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- Chưa có phản hồi hoặc đổi điểm: Chưa có phản hồi hoặc đổi điểm → Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-06-01 | Đối chiếu phép tính | Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3. |
| AC-RPT-06-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-06-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-06-04 | Chưa có phản hồi hoặc đổi điểm | Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-06-01 | AC-RPT-06-01 | Có điểm 5 và 3; yêu cầu chưa đánh giá không tính. |
| TC-RPT-06-02 | AC-RPT-06-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-06-03 | AC-RPT-06-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-06-04 | AC-RPT-06-04 | Chưa có phản hồi hoặc đổi điểm |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.

Master dùng parent `FR-RPT-06 | Tổng hợp mức hài lòng` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-05
