# Danh mục chức năng — đúng 4 module

| Mã module | Tên theo proposal | Số FR | Chi tiết |
| --- | --- | --- | --- |
| STU | Cổng hỗ trợ sinh viên | 5 | [01-student-support](01-student-support/README.md) |
| DSP | Điều phối và xử lý yêu cầu | 10 | [02-dispatch-processing](02-dispatch-processing/README.md) |
| RPT | Quản lý và báo cáo | 6 | [03-management-reporting](03-management-reporting/README.md) |
| IAM | Tài khoản và quyền truy cập | 3 | [04-accounts-permissions](04-accounts-permissions/README.md) |

| FR | Module | Hành vi | Actor | Nguồn |
| --- | --- | --- | --- | --- |
| FR-STU-01 | Cổng hỗ trợ sinh viên | Gửi yêu cầu hỗ trợ | Sinh viên | Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả |
| FR-STU-02 | Cổng hỗ trợ sinh viên | Xem danh sách yêu cầu của tôi | Sinh viên | Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả |
| FR-STU-03 | Cổng hỗ trợ sinh viên | Xem chi tiết và tiến độ yêu cầu | Sinh viên | Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả |
| FR-STU-04 | Cổng hỗ trợ sinh viên | Bổ sung thông tin được yêu cầu | Sinh viên | Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả |
| FR-STU-05 | Cổng hỗ trợ sinh viên | Phản hồi kết quả hỗ trợ | Sinh viên | Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả |
| FR-DSP-01 | Điều phối và xử lý yêu cầu | Xem hàng chờ điều phối | Điều phối viên | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-02 | Điều phối và xử lý yêu cầu | Xem hồ sơ xử lý và lịch sử nghiệp vụ | Điều phối viên / Nhân viên xử lý | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-03 | Điều phối và xử lý yêu cầu | Phân loại yêu cầu | Điều phối viên | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-04 | Điều phối và xử lý yêu cầu | Phân công trách nhiệm xử lý | Điều phối viên | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-05 | Điều phối và xử lý yêu cầu | Đặt hạn xử lý dự kiến | Điều phối viên | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-06 | Điều phối và xử lý yêu cầu | Bắt đầu xử lý yêu cầu | Nhân viên được giao | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-07 | Điều phối và xử lý yêu cầu | Yêu cầu sinh viên bổ sung thông tin | Nhân viên được giao | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-08 | Điều phối và xử lý yêu cầu | Ghi cập nhật tiến độ xử lý | Nhân viên được giao | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-09 | Điều phối và xử lý yêu cầu | Ghi kết quả giải quyết | Nhân viên được giao | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-DSP-10 | Điều phối và xử lý yêu cầu | Đóng yêu cầu đã giải quyết | Điều phối viên | Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử |
| FR-RPT-01 | Quản lý và báo cáo | Tổng hợp trạng thái yêu cầu | Quản lý | Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng |
| FR-RPT-02 | Quản lý và báo cáo | Theo dõi yêu cầu quá hạn | Quản lý | Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng |
| FR-RPT-03 | Quản lý và báo cáo | Thống kê thời gian xử lý | Quản lý | Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng |
| FR-RPT-04 | Quản lý và báo cáo | Thống kê vấn đề phổ biến | Quản lý | Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng |
| FR-RPT-05 | Quản lý và báo cáo | Thống kê khối lượng theo phòng ban | Quản lý | Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng |
| FR-RPT-06 | Quản lý và báo cáo | Tổng hợp mức hài lòng | Quản lý | Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng |
| FR-IAM-01 | Tài khoản và quyền truy cập | Đăng nhập | Sinh viên / Điều phối viên / Nhân viên / Quản lý | Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan |
| FR-IAM-02 | Tài khoản và quyền truy cập | Đăng xuất | Mọi vai trò đã đăng nhập | Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan |
| FR-IAM-03 | Tài khoản và quyền truy cập | Kiểm soát quyền theo vai trò và hồ sơ | Máy chủ cho mọi vai trò | Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan |

Các mục domain, workflow, NFR và architecture là tài liệu hỗ trợ, không phải module sản phẩm bổ sung.
