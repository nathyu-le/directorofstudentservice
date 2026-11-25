# Mục lục tài liệu UniSupport

1. Đọc [Proposal](../project_proposal.md) và [tổng quan sản phẩm](01-overview/scope.md) để hiểu vấn đề, actor, mục tiêu, phạm vi và phần loại trừ. Bản khách hàng xem có [Word](05-project/proposal.docx) và [hồ sơ theo phiên bản](05-project/sent/README.md).
2. Đọc [bốn module và 24 FR](03-modules/README.md) cùng [NFR](01-overview/non-functional-requirements.md). Xem [vòng đời](02-domain/lifecycle.md), [phân quyền](02-domain/permissions.md), [API](02-domain/api-contract.md) và [từ điển dữ liệu](02-domain/data-dictionary.md) để biết cách triển khai.
3. Đọc [test chức năng](04-acceptance/test-cases.md), [fixture](04-acceptance/fixtures.md), [test hệ thống](04-acceptance/system-tests.md) và [truy vết theo AC](04-acceptance/traceability-matrix.md). Xem [QA](../qc%26qa/QA.md) và [QC](../qc%26qa/QC.md) để biết bằng chứng, phân loại lỗi và cổng chất lượng.
4. Đọc [báo giá](05-project/quotation.xlsx), [giải thích Master Plan](05-project/master-alignment.md), [Master Plan Excel](../UniSupport_Master_Plan.xlsx) và [sổ rủi ro](05-project/risk-register.md). Xem [câu hỏi mở, giả định và tác động](05-project/open-questions.md).
5. Đọc [Team Charter](../TeamCharter.md), [quản trị và giám sát](../Quản%20trị%20giám%20sát.md), [quy trình CR](05-project/change-control.md), [phản hồi sản phẩm](06-product/feedback-process.md), [roadmap](06-product/roadmap.md) và [chỉ số giá trị](06-product/success-metrics.md).

## Tra nhanh nguồn gốc

| Nội dung | Nơi tra |
| --- | --- |
| Scope và non-goals | [Proposal mục 2](../project_proposal.md#2-phạm-vi-và-giả-định) |
| Nguồn lực | [Master Plan Excel](../UniSupport_Master_Plan.xlsx), sheet Resources. PM/BA phụ trách. |
| Mốc và task | [Manifest](05-project/requirements.json), `traceability[].milestone` và `plan.tasks`. Kế hoạch truy cập qua [GitHub Issues](https://github.com/nathyu-le/directorofstudentservice/issues) và [Milestones](https://github.com/nathyu-le/directorofstudentservice/milestones). |
| Báo giá | [Bảng tính](05-project/quotation.xlsx), sheets Hạng mục, Thanh toán. PM/BA phụ trách. |
| Word và PDF | [Bản theo phiên bản](05-project/sent/README.md). PM/BA giữ hồ sơ phát hành. |
| Rủi ro | [R-01 đến R-08](05-project/risk-register.md) |
| Change request | [Quy trình](05-project/change-control.md) và [mẫu issue](../.github/ISSUE_TEMPLATE/4-change-request.md) |
| Definition of Done | [QA mục 4.3](../qc%26qa/QA.md#43-definition-of-done-theo-loại-công-việc) |

## Đối chiếu mã với template

| Mẫu của thầy | Mã dùng tại UniSupport |
| --- | --- |
| M01–M04 | M01 = STU, M02 = DSP, M03 = RPT, M04 = IAM. |
| FR-STU-01/AC-01 | AC-STU-01-01. Đoạn STU-01 chỉ FR, hai số cuối là thứ tự AC trong FR. |
| TS-xx | TC-`<module>-<FR>-<AC>` đối với test chức năng. E2E-xx là kịch bản xuyên luồng, NFR-xx và MET-xx là nhóm kiểm/đo riêng. |
| Q-xx, MS-xx, R-xx, CR-xx | Cùng quy ước của template. Q-05 là công việc dùng chung và Q-06 là dự phòng. |

Các mã đã dùng trong issue, Excel và test được giữ để đường dẫn cũ còn truy vết được. Ma trận nối mã từng AC với test, parent issue, task QA, milestone và báo giá.

## Mô phỏng

Aurora University, bộ cấu hình quyền, dữ liệu thử, các phép đo MET, lịch/giờ dự kiến và đội thực thi bốn vị trí là mô phỏng. Nhóm 5 là ban quản lý hồ sơ dự án. Mọi kết quả chạy, phản hồi, ngày gửi và quyết định được cập nhật khi có bằng chứng.
