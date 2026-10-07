# Độ tin cậy

## NFR-REL-01 Tính toàn vẹn

**Yêu cầu:** Mọi thao tác nghiệp vụ ghi thành công lưu đủ Ticket, dữ liệu con, event và idempotency result; lỗi bất kỳ phần nào rollback. Claim, answer và feedback không tạo trùng khi đồng thời.

**Cách kiểm chứng:** Tiêm lỗi trước và sau các điểm ghi trong môi trường test; so dữ liệu trước/sau. Chạy hai request song song với cùng version và kiểm số bản, actor, trạng thái, lịch sử.

## NFR-REL-02 Phục hồi thông báo

**Yêu cầu:** Event đã commit được giữ đến khi xử lý đủ recipient; bộ xử lý retry được sau khởi động lại, không tạo trùng notification. Không để mất thông báo do tiến trình dừng.

**Cách kiểm chứng:** Dừng bộ xử lý sau commit, khởi động lại; đối chiếu event/recipient và số notification. Trong chế độ hoạt động bình thường, notification xuất hiện trong 60 giây sau event; quét quá hạn tối đa mỗi 60 giây.

## NFR-REL-03 Sao lưu và phục hồi prototype

**Yêu cầu:** Có bản sao database trước UAT và trước bàn giao; trong thử nghiệm phục hồi, khôi phục được dữ liệu từ bản sao gần nhất và chạy một luồng WF-01 đến WF-06.

**Cách kiểm chứng:** BE lưu dump và checksum; QA phục hồi ở database riêng, kiểm số Ticket, quan hệ, lịch sử và đăng nhập. Thời gian phục hồi được ghi thực tế, không tuyên bố SLA production.

[Về danh mục PRD](../README.md)
