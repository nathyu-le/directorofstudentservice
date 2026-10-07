# UniSupport PRD và bản đồ tài liệu

PRD 3.0 ngày 07 tháng 10 năm 2026 đặc tả UniSupport gồm 10 module, 35 chức năng, 194 tiêu chí nghiệm thu và 194 kịch bản theo tiêu chí, cùng 12 kịch bản đầu cuối. Đây là baseline đề nghị để PM, developer, QA và Sponsor review trước triển khai. Chưa có kết quả test hoặc phê duyệt Sponsor được ghi nhận.

Mã FR là nguồn liên kết sang Team Charter và Master Plan. Một chức năng có actor, điều kiện, dữ liệu, luồng, rule, lỗi và kết quả riêng; module và workflow không thay mã chức năng. Các mức SLA và giới hạn trường được áp dụng thống nhất, cần xác nhận trong review baseline.

## Cách đọc

1. Đọc phần sản phẩm để hiểu vấn đề, mục tiêu, vai trò và phạm vi.
2. Đọc miền nghiệp vụ, đặc biệt state-transition và business-rules trước đặc tả chức năng.
3. Dùng README của từng module để tìm FR; prd.md là đặc tả của các FR trong module.
4. Dùng workflow để đối chiếu luồng nhiều module và acceptance để kiểm thử.
5. Dùng kiến trúc và ADR khi triển khai; dùng phần dự án để chốt giả định và lịch.

## Các phần tài liệu
| Thư mục | Nội dung |
| --- | --- |
| 01-product | Tổng quan, vấn đề, mục tiêu, vai trò, phạm vi |
| 02-domain | Miền nghiệp vụ, thuật ngữ, mô hình, vòng đời, chuyển trạng thái, rule |
| 03-modules | 10 module, mỗi module có README.md và prd.md |
| 04-workflows | 6 quy trình đầu cuối |
| 05-non-functional-requirements | Hiệu năng, bảo mật, tin cậy, sử dụng, quan sát, triển khai |
| 06-acceptance | Matrix, dữ liệu và kịch bản kiểm thử |
| 07-architecture | Ngữ cảnh, kiến trúc, dữ liệu, API, bảo mật và 4 ADR |
| 08-project | Giả định, ràng buộc, điểm cần chốt, quy ước |

## Danh mục chức năng

| FR | Module | Tên chức năng | Actor | Ưu tiên |
| --- | --- | --- | --- | --- |
| FR-IAM-01 | M01 | Đăng nhập | SV, NV, QL | Must |
| FR-IAM-02 | M01 | Đăng xuất | SV, NV, QL | Must |
| FR-IAM-03 | M01 | Kiểm soát quyền truy cập | Hệ thống | Must |
| FR-STU-01 | M02 | Xem danh sách yêu cầu của tôi | SV | Must |
| FR-STU-02 | M02 | Sinh viên gửi yêu cầu hỗ trợ | SV | Must |
| FR-STU-03 | M02 | Xem chi tiết và tiến độ yêu cầu của tôi | SV | Must |
| FR-TKT-01 | M03 | Xem hàng chờ và tìm kiếm Ticket | NV, QL | Must |
| FR-TKT-02 | M03 | Điều chỉnh loại vấn đề của Ticket | QL | Must |
| FR-TKT-03 | M03 | Xem chi tiết Ticket nội bộ | NV, QL | Must |
| FR-TKT-04 | M03 | Xem lịch sử nghiệp vụ Ticket | SV, NV, QL | Must |
| FR-OPS-01 | M04 | Nhận Ticket từ hàng chờ | NV | Must |
| FR-OPS-02 | M04 | Bắt đầu xử lý Ticket được phân công | NV | Must |
| FR-OPS-03 | M04 | Ghi cập nhật tiến độ xử lý | NV | Must |
| FR-OPS-04 | M04 | Hoàn tất xử lý Ticket | NV | Must |
| FR-OPS-05 | M04 | Từ chối xử lý Ticket | NV | Must |
| FR-ASG-01 | M05 | Phân công người phụ trách Ticket | QL | Must |
| FR-ASG-02 | M05 | Đổi người phụ trách Ticket | QL | Must |
| FR-ASG-03 | M05 | Chuyển Ticket sang phòng ban khác | QL | Must |
| FR-COM-01 | M06 | Yêu cầu sinh viên bổ sung thông tin | NV | Must |
| FR-COM-02 | M06 | Sinh viên trả lời yêu cầu bổ sung | SV | Must |
| FR-COM-03 | M06 | Xem hội thoại bổ sung của Ticket | SV, NV, QL | Must |
| FR-NOT-01 | M07 | Tạo thông báo trong ứng dụng theo sự kiện | Hệ thống | Must |
| FR-NOT-02 | M07 | Xem danh sách thông báo của tôi | SV, NV, QL | Must |
| FR-NOT-03 | M07 | Đánh dấu một thông báo đã đọc | SV, NV, QL | Must |
| FR-SLA-01 | M08 | Tính hạn xử lý khi tạo Ticket | Hệ thống | Must |
| FR-SLA-02 | M08 | Điều chỉnh hạn xử lý Ticket | QL | Must |
| FR-SLA-03 | M08 | Xác định và hiển thị Ticket quá hạn | Hệ thống | Must |
| FR-FDB-01 | M09 | Sinh viên xác nhận đóng Ticket | SV | Must |
| FR-FDB-02 | M09 | Sinh viên gửi đánh giá mức hài lòng | SV | Must |
| FR-RPT-01 | M10 | Thống kê Ticket theo trạng thái | QL | Must |
| FR-RPT-02 | M10 | Xem danh sách Ticket quá hạn đang mở | QL | Must |
| FR-RPT-03 | M10 | Tính thời gian giải quyết thành công trung bình | QL | Must |
| FR-RPT-04 | M10 | Thống kê Ticket theo loại vấn đề | QL | Must |
| FR-RPT-05 | M10 | Thống kê tải công việc theo nhân viên | QL | Must |
| FR-RPT-06 | M10 | Thống kê mức hài lòng của sinh viên | QL | Must |


## Quyền đọc và nguồn yêu cầu

Trạng thái, rule và giới hạn trong bản này là chuẩn chung để các nhóm triển khai cùng hành vi. Bản trước được đối chiếu trong traceability-matrix; không dùng song song hai bộ mã. Bản Word tổng hợp cùng nội dung của bộ Markdown này để review thuận tiện. Nếu thay đổi, cập nhật cả hai biểu diễn từ cùng nguồn trước khi phát hành phiên bản.

[01-product](01-product/product-overview.md) · [02-domain](02-domain/domain-overview.md) · [03-modules](03-modules/README.md) · [04-workflows](04-workflows/README.md) · [05-non-functional-requirements](05-non-functional-requirements/README.md) · [06-acceptance](06-acceptance/README.md) · [07-architecture](07-architecture/README.md) · [08-project](08-project/assumptions.md)
