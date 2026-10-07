# [FR-DSP-08] Ghi cập nhật tiến độ xử lý

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Ghi công việc đã thực hiện để phối hợp và theo dõi.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| content | Bắt buộc | Không rỗng; CFG-02. |
| visibility | Bắt buộc | public hoặc internal; mặc định internal. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên nhập tiến độ và chọn công khai hoặc nội bộ.
2. Máy chủ kiểm tra quyền, trạng thái và nội dung.
3. Thêm sự kiện tiến độ có người ghi, thời gian và mức hiển thị.
4. Giữ trạng thái/trách nhiệm; chỉ nội dung công khai hiện cho sinh viên.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Giá trị sai: Nội dung trống hoặc visibility=secret. → Từ chối; không thêm lịch sử.
- Mất trách nhiệm: NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. → Từ chối; không ghi nội dung mới.
- Tiến độ nội bộ: Ghi ghi chú internal có dấu TEST-INTERNAL. → Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-08-01 | Tiến độ công khai | Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi. |
| AC-DSP-08-02 | Giá trị sai | Từ chối; không thêm lịch sử. |
| AC-DSP-08-03 | Mất trách nhiệm | Từ chối; không ghi nội dung mới. |
| AC-DSP-08-04 | Tiến độ nội bộ | Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-08-01 | AC-DSP-08-01 | Ghi Đang kiểm tra thủ tục, visibility=public. |
| TC-DSP-08-02 | AC-DSP-08-02 | Nội dung trống hoặc visibility=secret. |
| TC-DSP-08-03 | AC-DSP-08-03 | NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. |
| TC-DSP-08-04 | AC-DSP-08-04 | Ghi ghi chú internal có dấu TEST-INTERNAL. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.

Master dùng parent `FR-DSP-08 | Ghi cập nhật tiến độ xử lý` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-06
