# Quy tắc nghiệp vụ đề xuất

| Mã | Quy tắc | FR |
| --- | --- | --- |
| BR-01 | Kiểm tra quyền máy chủ trước đọc/ghi, gồm quyền hành động và đối tượng. | IAM-03 |
| BR-02 | Một mã/đối tượng cho một lần gửi; gửi lặp cùng submission_key không tạo trùng. | STU-01 |
| BR-03 | Một bộ trách nhiệm hiện hành; người thuộc đơn vị; giao lại giữ lịch sử và chuyển quyền. | DSP-04 |
| BR-04 | Chỉ chuyển theo bảng trạng thái; các thao tác phụ không âm thầm đổi trạng thái. | DSP-06/07/09/10, STU-04 |
| BR-05 | Lịch sử không xóa; công khai và nội bộ được lọc cả phía máy chủ. | STU-03, DSP-02/08 |
| BR-06 | Ghi dữ liệu nghiệp vụ và lịch sử cùng giao dịch; stale version bị chặn. | Các FR ghi |
| BR-07 | Quá hạn: còn mở, có due_at và now > due_at; không cộng hồ sơ đã kết thúc. | DSP-05, RPT-02 |
| BR-08 | Kỳ nửa mở [đầu ngày from, đầu ngày sau to), theo CFG-06. RPT-03 chọn resolved_at; RPT-06 chọn feedback.updated_at; báo cáo khác chọn created_at. Hiển thị rõ định nghĩa kỳ. | RPT-01..06 |
| BR-09 | Một phản hồi hiện hành mỗi yêu cầu; sửa phản hồi không tăng mẫu; không đánh giá hồ sơ chưa có kết quả. | STU-05, RPT-06 |
| BR-10 | Nguồn báo cáo dùng cùng quyền/bộ lọc; đối chiếu tổng với danh sách nguồn, nêu Không có dữ liệu khi mẫu bằng 0. | RPT-01..06 |

BR là chi tiết cụ thể hóa proposal để làm prototype, chưa phải quy định vận hành được trường xác nhận. Không suy ra SLA số ngày hoặc quyền quyết định học vụ từ đây.
