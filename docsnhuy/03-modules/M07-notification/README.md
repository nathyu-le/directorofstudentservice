# M07 Thông báo

**Ranh giới:** Tạo, đọc và đánh dấu thông báo trong ứng dụng; không gửi email hoặc SMS.

| FR | Tên | Actor |
| --- | --- | --- |
| FR-NOT-01 | Tạo thông báo trong ứng dụng theo sự kiện | Hệ thống |
| FR-NOT-02 | Xem danh sách thông báo của tôi | SV, NV, QL |
| FR-NOT-03 | Đánh dấu một thông báo đã đọc | SV, NV, QL |

## Sự kiện và người nhận

| Event | FR nguồn | Người nhận |
| --- | --- | --- |
| TICKET_CREATED | FR-STU-02 | SV chủ Ticket; QL của phòng ban tiếp nhận |
| TICKET_CLAIMED | FR-OPS-01 | SV; QL phòng ban hiện tại |
| ASSIGNEE_ASSIGNED | FR-ASG-01 | NV được giao; SV |
| ASSIGNEE_CHANGED | FR-ASG-02 | NV cũ; NV mới; SV |
| DEPARTMENT_TRANSFERRED | FR-ASG-03 | SV; NV cũ nếu có; QL nguồn; QL đích |
| PROGRESS_ADDED | FR-OPS-03 | SV |
| INFORMATION_REQUESTED | FR-COM-01 | SV |
| INFORMATION_ANSWERED | FR-COM-02 | NV phụ trách hiện tại |
| TICKET_RESOLVED hoặc TICKET_REJECTED | FR-OPS-04 hoặc FR-OPS-05 | SV |
| DUE_DATE_CHANGED | FR-SLA-02 | SV; NV phụ trách nếu có |
| OVERDUE_DETECTED | FR-SLA-03 | NV phụ trách nếu có; QL hiện tại |
| TICKET_CLOSED | FR-FDB-01 | NV phụ trách nếu có |
| FEEDBACK_SUBMITTED | FR-FDB-02 | NV phụ trách nếu có; QL hiện tại |

[Về danh mục PRD](../../README.md)
