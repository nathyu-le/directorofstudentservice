# WF-03 Yêu cầu và trả lời bổ sung thông tin

**Actor:** NV phụ trách; SV chủ Ticket

**Preconditions:** Ticket PROCESSING của NV, không có câu hỏi OPEN.

## Luồng chính

1. FR-COM-01 — NV gửi câu hỏi cụ thể; một OPEN, chuyển WAITING_INFO.
2. FR-NOT-01, FR-STU-03 — SV nhận thông báo và thấy đúng câu hỏi đang mở.
3. FR-COM-02 — SV gửi một câu trả lời; lưu answer, câu hỏi ANSWERED, chuyển PROCESSING.
4. FR-NOT-01 — Thông báo cho NV phụ trách hiện tại.
5. FR-COM-03 — Các bên có quyền đọc lại các vòng hỏi trả lời; không có chat ngoài vòng bổ sung.

## Kết quả cuối

Cùng mã Ticket, một câu trả lời cho vòng hỏi; Ticket trở về PROCESSING; hạn không bị dừng hoặc đặt lại.

## Luồng thay thế và lỗi

SV khác, câu hỏi sai Ticket, nội dung trống hoặc version cũ bị chặn. Đổi người khi đang chờ giữ câu hỏi; SV tải lại rồi trả lời cho người mới. Không chuyển phòng ban khi WAITING_INFO.

## Kiểm thử đầu cuối

E2E-05.

[Về danh mục PRD](../README.md)
