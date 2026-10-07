# Phạm vi kiểm thử và điều kiện nghiệm thu

QA kiểm từng AC của 35 FR, các NFR và sáu workflow đầu cuối. Một tiêu chí bắt buộc chưa đạt thì chức năng chưa Done; kết quả mục tiêu 90% không được dùng để bỏ qua lỗi quyền, dữ liệu, chống trùng hoặc AC. Kịch bản TS trong tài liệu đã chỉ rõ hành động thử và kết quả; QA triển khai test case có bước thao tác, dữ liệu cụ thể, actual result, status và bằng chứng trên môi trường đã chốt.

Trạng thái kiểm thử gồm Not run, Pass, Fail, Blocked. Khi chưa chạy, ghi Not run; không điền Pass chỉ vì đã có đặc tả. Ca Blocked cần lý do, owner và điều kiện gỡ chặn. Bug phải có mã FR/AC, môi trường, version build, bước tái hiện, expected, actual và bằng chứng; retest dùng build đã sửa và regression phần có tác động.

Điều kiện nghiệm thu: toàn bộ AC và NFR bắt buộc đạt; sáu workflow chạy qua cả nhánh giải quyết và từ chối; không có lỗi chặn hoặc lỗi nghiêm trọng về quyền, mất/trùng dữ liệu, sai kết quả; lỗi nhỏ còn lại nếu có phải được Sponsor ghi nhận chấp thuận với kế hoạch sửa. Có báo cáo kiểm thử, traceability, source và seed, hướng dẫn cài, job, backup và phục hồi. Sponsor kiểm kết quả UAT thực tế, không phê duyệt chỉ dựa trên số trang tài liệu.

Sev-1 là rò dữ liệu, mất/trùng dữ liệu hoặc toàn bộ luồng cốt lõi không thể tiếp tục; Sev-2 là hành vi hoặc chỉ số bắt buộc sai, làm hỏng một nhánh nghiệp vụ chính; Sev-3 là lỗi cục bộ có cách đi tiếp mà dữ liệu đúng; Sev-4 là trình bày hoặc nội dung không ảnh hưởng hành vi. PM và QA thống nhất severity theo tác động, không theo người phát hiện.

Ma trận truy vết nối FR, module, BR, workflow, AC và TS. NFR có phương pháp kiểm chứng tại phần 5 và mã kiểm NT tương ứng trong matrix. Kết quả thực thi được lưu riêng theo build; PRD không ghi kết quả kiểm thử chưa thực hiện.

[Về danh mục PRD](../README.md)
