# Đề xuất dự án UniSupport

**Phiên bản 6.2 · 25/11/2026 · Bản công bố để review trong repo (Mô phỏng)**

**Bên đề xuất:** Nhóm Solo Leveling. **Khách hàng:** Aurora University (Mô phỏng).

## 1. Bài toán hiện tại và mục tiêu

Trong kịch bản mô phỏng, sinh viên gửi yêu cầu qua email hoặc trực tiếp tại phòng ban. Khi cần bổ sung hay hỏi tiến độ, sinh viên liên hệ lại vì thiếu mã theo dõi chung. Nhân viên chuyển tiếp yêu cầu giữa các phòng ban nên khó biết ai đang phụ trách và dễ xử lý trùng. Quản lý tổng hợp thủ công nên khó thấy hồ sơ quá hạn. Nhóm dùng kịch bản này để thử nghiệm và chưa có số liệu vận hành của trường.

UniSupport là prototype web PHP/MySQL giúp sinh viên tra cứu, nhân viên xử lý trên cùng hồ sơ và quản lý đối chiếu báo cáo. Mục tiêu mô phỏng là ≥90% hồ sơ có người chịu trách nhiệm, ≥90% hồ sơ có trạng thái theo dõi được và giảm ≥30% lượt hỏi lại. Mức giảm là (B − A) / B với B > 0, dùng số lượt hỏi trong hai lượt thử tương đương trước và sau khi sử dụng UniSupport. Hồ sơ bỏ sót hoặc trùng được đo riêng. Xem [MET-01 đến MET-04](docs/06-product/success-metrics.md).

## 2. Phạm vi và giả định

| Mã | Phần giao |
| --- | --- |
| M01 | Sinh viên gửi yêu cầu, tự mở cổng xem tiến độ, đọc và trả lời câu hỏi bổ sung, phản hồi kết quả. |
| M02 | Điều phối phân loại, phân công, đặt hạn và đóng hồ sơ. Nhân viên cập nhật tiến độ, hỏi bổ sung và ghi kết quả. |
| M03 | Sáu báo cáo nội bộ về trạng thái, quá hạn, thời gian xử lý, loại vấn đề, khối lượng và mức hài lòng. |
| M04 | Đăng nhập, đăng xuất và kiểm quyền trên máy chủ. Tài khoản, danh mục và quyền lấy từ dữ liệu khởi tạo. |

Ngoài phạm vi gồm vận hành toàn trường, tích hợp hệ thống thật, dữ liệu cá nhân thật, học phí, quyết định học vụ tự động, kho tri thức/FAQ, tải tệp, xuất báo cáo ra tệp, email/SMS/thông báo tự động, công cụ tự tính hạn dịch vụ, màn hình quản trị tài khoản/danh mục/quyền và đặt lại mật khẩu. Người có quyền cấu hình cập nhật dữ liệu khởi tạo trên môi trường thử khi đầu mối khách hàng xác nhận. Xem [module](docs/03-modules/README.md) và [câu hỏi cần xác nhận](docs/05-project/open-questions.md).

## 3. Tiến độ theo mốc

Lịch dự kiến 15 tuần từ 12/10/2026 đến 22/01/2027. Đội thực thi **Mô phỏng** có một người cho mỗi vị trí PM/BA, Backend, Frontend và QA/tài liệu. Lịch, nguồn lực và effort cần xác nhận theo OQ-01, OQ-05 và OQ-08. [Master Plan](docs/05-project/master-alignment.md) mô tả công việc và tiêu chí từng mốc.

| Mốc | Ngày dự kiến | Khách tự dùng thử |
| --- | --- | --- |
| MS-01 | 06/11/2026 | Đăng nhập, đăng xuất và kiểm quyền bằng tài khoản mẫu. |
| MS-02 | 16/11/2026 | Gửi hồ sơ, đọc tiến độ và đối chiếu cùng mã trong hàng chờ. |
| MS-03 | 04/12/2026 | Phân loại, giao người, đặt hạn, cập nhật và trao đổi bổ sung. |
| MS-04 | 09/12/2026 | Ghi và đọc kết quả, phản hồi rồi đóng hồ sơ. |
| MS-05 | 18/12/2026 | Dùng sáu báo cáo và đối chiếu số liệu với dữ liệu mẫu. |
| MS-06 | 22/01/2027 | Người tiếp nhận cài lại bản bàn giao và chạy kịch bản xuyên suốt. |

## 4. Báo giá và thanh toán

Giá chào đề xuất **300.000.000 đồng** theo [bảng tính Q-01 đến Q-06](docs/05-project/quotation.xlsx). Đơn giá chào dùng để lập dự toán là 937.500 đồng/giờ. Mức này được xác lập theo giá và effort của bản 6.1, sau đó giữ cố định làm đầu vào tính giá của bản 6.2. Dự toán gồm 288 giờ kế hoạch và 32 giờ dự phòng. Khi effort thay đổi, nhân số giờ thay đổi với đơn giá ghi trong báo giá. Mọi đề nghị đổi đơn giá được xem xét riêng trong CR. Báo giá bao gồm phân tích, phát triển, tích hợp, kiểm thử, sửa lỗi và bàn giao. Dự phòng tính một lần. Giá dựa trên phạm vi bốn module, dữ liệu mô phỏng, PHP/MySQL và lịch nguồn lực dự kiến. Thuế và chi phí bên ngoài cần xác nhận khi duyệt. Nếu phạm vi hoặc lịch thay đổi, áp dụng [quy trình CR](docs/05-project/change-control.md) để tính lại trước khi cam kết.

| Đợt | Điều kiện đề xuất | Tỷ lệ | VNĐ |
| --- | --- | --- | --- |
| 1 | MS-01 được xác nhận qua thử đăng nhập, đăng xuất và quyền. | 30% | 90.000.000 |
| 2 | MS-02 được xác nhận qua luồng gửi và theo dõi hồ sơ. | 30% | 90.000.000 |
| 3 | MS-05 được xác nhận qua bản tích hợp và sáu báo cáo, kế thừa MS-03 và MS-04. | 30% | 90.000.000 |
| 4 | MS-06 được xác nhận qua hồ sơ bàn giao và bằng chứng người nhận chạy lại. | 10% | 30.000.000 |

## 5. Rủi ro và quyết định

[Sổ rủi ro](docs/05-project/risk-register.md) theo dõi thay đổi yêu cầu, phân bổ nguồn lực, sai quyền hoặc dữ liệu và khả năng chạy lại. PM/BA ghi tác động và trình đại diện được ủy quyền của Phòng Công tác Sinh viên Aurora University (Mô phỏng) quyết định thay đổi phạm vi hoặc cam kết. Danh tính đầu mối cần xác nhận.

## 6. Nghiệm thu và bàn giao

Từng mốc dùng bản dựng có mã phiên bản, tài khoản mô phỏng, kịch bản dùng thử và [tiêu chí Master Plan](docs/05-project/master-alignment.md). Đại diện nghiệp vụ thử và ghi phản hồi, QA lưu kết quả thực thi và bằng chứng. Sai quyền, sai/mất dữ liệu hoặc luồng chính bị chặn cần sửa và kiểm lại. Lỗi còn lại cần quyết định chấp nhận có ghi rõ tác động.

Tại MS-06 người tiếp nhận cài môi trường sạch từ mã nguồn, cấu hình mẫu, dữ liệu khởi tạo và hướng dẫn, rồi lưu kết quả E2E-08. PM/BA trình bằng chứng kỹ thuật và hồ sơ bàn giao để đại diện được ủy quyền quyết định nghiệm thu. Nếu MET dưới mục tiêu hoặc thiếu mẫu, QA ghi số đo, mẫu và đề xuất xử lý để đại diện quyết định trước khi ký. Hồ sơ này được công bố trong repo để rà soát (Mô phỏng). Chưa có gửi khách thật, xác nhận hoặc chữ ký.
