# WF-06 Sinh viên đóng Ticket và đánh giá

**Actor:** SV chủ Ticket; hệ thống; QL đọc báo cáo

**Preconditions:** Ticket RESOLVED hoặc REJECTED thuộc SV.

## Luồng chính

1. FR-STU-03 — SV đọc kết quả và quyết định xác nhận đóng.
2. FR-FDB-01 — Xác nhận, chuyển CLOSED, giữ outcome và ended_at, ghi closed_at.
3. FR-FDB-02 — SV có thể gửi một điểm 1 đến 5 và nhận xét; đánh giá không bắt buộc để đóng.
4. FR-NOT-01 — Thông báo theo sự kiện đóng và feedback.
5. FR-RPT-01, FR-RPT-03, FR-RPT-06 — Báo cáo dùng CLOSED cho trạng thái, outcome cho thời gian thành công, feedback cho mức hài lòng.

## Kết quả cuối

Ticket CLOSED, có hoặc chưa có feedback; kết quả xử lý cũ được giữ.

## Luồng thay thế và lỗi

Không đóng Ticket còn mở. Không đánh giá trước CLOSED. Không mở lại hoặc sửa feedback. SV chưa xác nhận thì RESOLVED/REJECTED giữ nguyên, không có tự đóng.

## Kiểm thử đầu cuối

E2E-07, E2E-08, E2E-12.

[Về danh mục PRD](../README.md)
