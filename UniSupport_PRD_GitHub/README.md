# UniSupport PRD v5.1

PRD cho prototype PHP/MySQL của Aurora University. Phạm vi gốc: UniSupport_Project_Proposal.docx v6. Tài liệu này là bản đặc tả để nhóm review, không ghi nhận nghiệm thu hay vận hành thật.

## Đọc theo thứ tự

1. [Phạm vi và căn cứ proposal](docs/01-overview/scope.md).
2. [Vòng đời](docs/02-domain/lifecycle.md), [quyền](docs/02-domain/permissions.md), [dữ liệu](docs/02-domain/data-dictionary.md), [hợp đồng API](docs/02-domain/api-contract.md).
3. [Danh mục đúng4 module và từng chức năng](docs/03-modules/README.md).
4. [Test case](docs/06-acceptance/test-cases.md), [traceability](docs/06-acceptance/traceability-matrix.md), [E2E/NFR/mục tiêu](docs/06-acceptance/system-tests.md).
5. [Câu hỏi chưa chốt](docs/08-project/open-questions.md), [quy tắc đồng bộ Master](docs/08-project/master-alignment.md).

Mỗi FR có đúng form9 mục như ảnh tham khảo. Ảnh chỉ định hình cách trình bày, không là nguồn chức năng. JSON requirements là manifest của phiên bản, không thay đặc tả Markdown.

## Đưa lên GitHub

Giải nén ZIP, đưa thư mục docs và README vào repository của nhóm qua upload hoặc commit. Không ghi như đã push nếu chưa thao tác trên repository thật. Không đưa dữ liệu sinh viên thật, mật khẩu hay secret vào repository. Review phạm vi trước khi dùng làm baseline đã duyệt.

## Phạm vi cập nhật v5.1

Phiên bản 5.1 cập nhật baseline giờ, danh mục task kế hoạch và cách dùng Master Gantt một sheet. Đặc tả hành vi/API giữ nguyên revision thiết kế v5.0; không thêm/bỏ chức năng hoặc test case. Trần 288h chính + 32h dự phòng đã chốt, phân bổ giờ theo task vẫn cần nhóm review.
