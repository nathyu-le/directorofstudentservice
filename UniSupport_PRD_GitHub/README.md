# UniSupport — PRD v4.0

PRD được dựng lại từ **UniSupport_Project_Proposal.docx, phiên bản 6**, theo đúng **4 module** trong §2.1. Ảnh thầy dùng làm mẫu cây thư mục và cách mô tả chức năng; nội dung/module trong ảnh không được coi là phạm vi dự án.

Tài liệu ở trạng thái **dự thảo để rà soát**. Phạm vi, mục tiêu và mốc 15 tuần lấy từ proposal; trạng thái chi tiết, trường dữ liệu, cách phân quyền và cấu hình prototype là thiết kế đề xuất, có ghi rõ tại assumptions/open-questions. Không khẳng định khách hàng/thầy đã duyệt các chi tiết đó.

Đọc [mục lục](docs/README.md), [phạm vi](docs/01-product/product-scope.md), [danh mục chức năng](docs/03-modules/README.md), [truy vết](docs/06-acceptance/traceability-matrix.md), [test case](docs/06-acceptance/test-cases.md). `docs/08-project/requirements.json` là nguồn mã thống nhất cho Team Charter và Master; không dùng danh mục 10 module cũ.

Có **24 chức năng**, mỗi mã mô tả một hành vi; 96 tiêu chí và 96 test case chức năng, kèm 8 kịch bản xuyên module, 6 kiểm tra phi chức năng và 4 bài đo mục tiêu. Chưa có test nào được tuyên bố Pass.

## Đưa lên GitHub

1. Giải nén ZIP và mở thư mục UniSupport_PRD_GitHub.
2. Đưa README.md và toàn bộ docs/ vào cùng repository của nhóm; giữ đường dẫn tương đối.
3. Khi thay đổi chức năng, sửa file FR và requirements.json, cập nhật AC/TC và Master liên quan trong cùng lần review.
4. ZIP là bộ tài liệu sẵn để đưa lên GitHub; việc xuất ZIP không đồng nghĩa đã tạo repository hoặc push lên tài khoản của nhóm.
