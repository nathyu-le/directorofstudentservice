# Quy tắc nghiệp vụ dùng chung

## BR-01 Quyền truy cập

Mọi lần đọc và ghi kiểm quyền tại máy chủ theo vai trò và quan hệ hiện hành. SV chỉ Ticket của mình; NV chỉ Ticket đang giao cho mình hoặc RECEIVED chưa phân công trong phòng ban mình; QL chỉ Ticket thuộc phòng ban quản lý. Thông báo thuộc recipient, không tự cấp quyền Ticket.

## BR-02 Trách nhiệm xử lý

Một Ticket có tối đa một assignee hiện tại. NV chỉ xử lý Ticket giao cho mình. QL điều phối, không xử lý thay NV. NV được nhận Ticket chưa phân công trong cùng phòng ban. Không có nhóm đồng xử lý trong phiên bản này.

## BR-03 Khởi tạo Ticket

Loại vấn đề hoạt động xác định phòng ban tiếp nhận; cấu hình SLA hợp lệ là bắt buộc. Máy chủ xác định student_id, created_at, mã Ticket và trạng thái RECEIVED. Nội dung ban đầu không được sửa sau gửi.

## BR-04 Trạng thái

Chỉ sáu trạng thái RECEIVED, PROCESSING, WAITING_INFO, RESOLVED, REJECTED, CLOSED. Chưa phân công và Quá hạn là thuộc tính, không là trạng thái. Các chuyển hợp lệ chỉ có trong bảng ST-01 đến ST-09. Không có chuyển tự động khi xem trang.

## BR-05 Điều phối

Phân công lần đầu giữ RECEIVED; đổi người giữ trạng thái, hạn và câu hỏi mở. Điều chỉnh loại chỉ trong cùng phòng ban, khi RECEIVED hoặc PROCESSING. Mọi thay đổi điều phối có actor, thời điểm, giá trị trước sau và lý do nếu yêu cầu.

## BR-06 Chuyển phòng ban

QL có quyền nguồn được chuyển sang phòng ban đích hoạt động có QL hoạt động, kể cả không quản lý đích. Chọn loại hợp lệ ở đích, xóa phân công, đưa RECEIVED; giữ mã, nội dung gốc, created_at, started_at và due_at. Không chuyển WAITING_INFO hoặc Ticket có kết quả. Sau commit quyền nguồn không còn hiệu lực nếu không có quyền đích.

## BR-07 Bổ sung và kết thúc

Tối đa một câu hỏi bổ sung OPEN trên Ticket. Mỗi câu hỏi có tối đa một câu trả lời. Hoàn tất hoặc từ chối chỉ từ PROCESSING, không còn câu hỏi mở; ended_at và outcome được ghi một lần. SV đóng Ticket đã có kết quả; CLOSED giữ outcome. Mỗi Ticket CLOSED có tối đa một đánh giá, không sửa. Không mở lại hoặc tự đóng theo thời gian.

## BR-08 Giao dịch và lịch sử

Thay đổi Ticket, dữ liệu con, audit event, idempotency result và sự kiện thông báo phải commit hoặc rollback cùng nhau. Lịch sử chỉ thêm, không sửa hoặc xóa bằng nghiệp vụ. Thông báo được xử lý sau commit; lỗi xử lý thông báo không rollback Ticket đã commit.

## BR-09 Phiên bản đồng thời

Ticket khởi tạo version 1. Mỗi thao tác nghiệp vụ ghi thành công tăng đúng một version. Yêu cầu ghi kiểm version đã đọc và điều kiện hiện hành; version cũ trả xung đột, không ghi đè. Quét quá hạn, đọc trang và đánh dấu notification không tăng version Ticket. Điều kiện nhận Ticket được kiểm nguyên tử trong cùng giao dịch.

## BR-10 Chống gửi trùng

Mọi thao tác ghi Ticket sử dụng operation_id UUID theo actor và action; phạm vi còn gồm ticket_id trừ lúc tạo. Sau xác thực, tra kết quả đã commit thuộc đúng actor và action trước kiểm version; cùng mã và payload trả xác nhận tối thiểu, không kèm nội dung ngoài quyền hiện tại. Với thao tác mới, kiểm toàn bộ quyền hiện hành trước ghi. Payload khác trả xung đột. Hai operation_id khác nhau vẫn chịu khóa nghiệp vụ: một người nhận, một answer/câu hỏi, một feedback/Ticket. Giữ khóa chống trùng trong suốt thời gian lưu Ticket của prototype.

## BR-11 Xử lý dữ liệu

Trim hai đầu chuỗi trừ mật khẩu; độ dài tính theo ký tự Unicode. Lưu nội dung như văn bản và escape khi hiển thị. Máy chủ là nguồn thời gian, lưu UTC, hiển thị Asia/Ho_Chi_Minh. Phiên hết hạn sau 30 phút không hoạt động; thời hạn kiểm bằng máy chủ.

## BR-12 Danh sách và lọc

Áp quyền trước lọc và phân trang; truy vấn đếm và dữ liệu cùng điều kiện. Kích thước trang 20; page nguyên từ 1, quá trang cuối trả tập rỗng. Thứ tự bổ sung bằng khóa duy nhất khi cùng timestamp. Không cam kết snapshot qua nhiều trang nếu dữ liệu thay đổi giữa hai lần tải.

## BR-13 Thông báo

Một event_id và recipient_id tạo tối đa một notification. Người nhận được chụp ở thời điểm commit, hợp nhất trùng và bỏ tài khoản không hoạt động. Nội dung thông báo chỉ có mã Ticket và hành động. Mở Ticket phải kiểm quyền hiện tại. Notification giữ read_at lần đầu; danh sách không tự đánh dấu đã đọc.

## BR-14 SLA

due_at = created_at + sla_hours_snapshot giờ liên tục; SLA mẫu 48 giờ, cấu hình nguyên 1 đến 168. Không dừng khi WAITING_INFO, không bỏ cuối tuần. Đổi loại, người hoặc phòng ban không tính lại hạn. QL đổi hạn chỉ khi đang mở, có lý do, không trước created_at hoặc thời điểm lưu. Quá hạn đang mở khi as_of_at > due_at, mỗi hạn cụ thể chỉ cảnh báo một lần.

## BR-15 Báo cáo

Tập báo cáo lọc created_at theo ngày Việt Nam và department_id hiện tại trong quyền QL. Một lần chạy có một as_of_at và snapshot nhất quán. Đếm mỗi ticket_id một lần. RESOLVED và REJECTED là trạng thái trước đóng; outcome giữ kết quả sau CLOSED. Trung bình không mẫu là NULL; số đếm rỗng là 0. Làm tròn một chữ số thập phân ở bước hiển thị cuối.

[Về danh mục PRD](../README.md)
