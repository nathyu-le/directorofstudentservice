# Tổng quan sản phẩm UniSupport

## Vấn đề

[Proposal mục 1](../../project_proposal.md#1-bài-toán-hiện-tại-và-mục-tiêu) mô tả hiện trạng Aurora University (Mô phỏng). Sinh viên thiếu mã theo dõi xuyên suốt, nhân viên khó biết hồ sơ thuộc ai sau khi chuyển phòng, còn quản lý cần nguồn chung để kiểm tra tình trạng và hồ sơ quá hạn.

## Người dùng

| Actor | Quyền và nhu cầu |
| --- | --- |
| Sinh viên | Gửi, đọc hồ sơ của mình, bổ sung và phản hồi. |
| Nhân viên xử lý | Xem hồ sơ được giao, hỏi bổ sung, ghi tiến độ và kết quả. |
| Điều phối | Quyền bổ sung cấp cho nhân viên hoặc quản lý trong phạm vi xác định. Xem hàng chờ, phân loại, phân công, đặt hạn và đóng hồ sơ. |
| Quản lý | Xem báo cáo theo phạm vi được cấp. Quyền đọc chi tiết cần cấp riêng. |

Xem [ma trận quyền](../02-domain/permissions.md).

## Goals và non-goals

- G1. Hồ sơ có mã, trạng thái và người phụ trách để sinh viên tự theo dõi. Liên quan M01, M02 và MET-01 đến MET-04.
- G2. Đổi đơn vị hoặc bổ sung thông tin trên cùng hồ sơ, giữ lịch sử. Liên quan M01, M02.
- G3. Đối chiếu báo cáo theo phạm vi được cấp. Liên quan M03, M04.
- G4. Kiểm quyền trên máy chủ và đóng gói bản chạy lại được trên dữ liệu mô phỏng. Liên quan M04, NFR-SEC-01 và NFR-DEP-01.
- Non-goals theo [ranh giới tại Proposal mục 2](../../project_proposal.md#2-phạm-vi-và-giả-định). Xem [hồ sơ Proposal có phiên bản và ngày](../05-project/sent/README.md).

## Ưu tiên yêu cầu

P0 giữ luồng tối thiểu và bảo vệ quyền/dữ liệu. P1 hỗ trợ phối hợp hoặc quyết định quản lý. P2 là phân tích bổ trợ, xem xét lùi trước P1 khi thiếu nguồn lực. Cả ba nhóm đều trong phạm vi đề xuất. Đổi mốc hoặc phạm vi phải đi qua [CR](../05-project/change-control.md).
