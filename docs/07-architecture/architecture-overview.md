# Kiến trúc triển khai đề nghị

Ứng dụng dùng kiến trúc một khối có module. Controller nhận request và validate hình thức; middleware xác thực phiên, CSRF và quyền cơ bản; domain service kiểm điều kiện Ticket và thực hiện transaction; repository dùng truy vấn tham số; view/response chỉ trả dữ liệu trong quyền. Mọi màn hình và endpoint dùng cùng domain service để tránh quy tắc chỉ tồn tại trên UI.

Quy trình ghi: xác thực, kiểm operation_id của actor để xử lý retry đã commit; với thao tác mới kiểm quyền và version, khóa bản ghi cần thiết, kiểm trạng thái/assignee/câu hỏi mở; cập nhật Ticket và dữ liệu con; thêm audit event và event thông báo với recipient snapshot; lưu kết quả chống trùng; commit. Response chỉ báo thành công sau commit. Khi rollback, không để event hoặc kết quả chống trùng báo thành công.

Job thông báo đọc các event chưa xử lý đủ, insert notification theo khóa event/recipient rồi ghi trạng thái xử lý. Nếu dừng giữa các recipient, lần chạy sau tiếp tục phần thiếu. Job SLA khóa hoặc kiểm nguyên tử trạng thái mở và due_at hiện hành trước tạo OVERDUE_DETECTED; không cảnh báo một Ticket đã kết thúc giữa quét và ghi. Job có khóa chạy để tránh các lần quét tự cạnh tranh; khóa nghiệp vụ vẫn bảo đảm đúng nếu job bị chạy trùng.

Mỗi lần chạy báo cáo dùng một transaction đọc nhất quán hoặc cách tương đương được BE chứng minh, cùng as_of_at và tập lọc. Chỉ số dùng dữ liệu hiện tại tại lần đọc, không truy dựng trạng thái lịch sử ở một ngày quá khứ. as_of_at là mốc chụp của lần chạy, không là tham số tùy ý từ người dùng; trong test dùng clock có kiểm soát.

Danh mục và seed thuộc dữ liệu cấu hình. Các giới hạn trường và enum dùng chung BE/FE; BE là nguồn kiểm cuối. Giao diện có thể polling để nhận cập nhật/notification; không cần websocket. Log correlation_id liên kết request, transaction, event và job theo NFR-OBS-01.

[Về danh mục PRD](../README.md)
