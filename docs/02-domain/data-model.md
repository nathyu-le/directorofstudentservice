# Mô hình Dữ liệu (Data Model / ERD)

Dưới đây là sơ đồ Thực thể - Liên kết (Entity Relationship Diagram) sơ bộ để team Backend thiết kế cơ sở dữ liệu MySQL.

## Sơ đồ ERD

```mermaid
erDiagram
    USERS ||--o{ TICKETS : creates
    USERS ||--o{ TICKETS : is_assigned_to
    DEPARTMENTS ||--|{ USERS : belongs_to
    TICKETS ||--o{ AUDIT_LOGS : has
    TICKETS ||--o{ ATTACHMENTS : contains
    TICKETS ||--o| FEEDBACKS : receives

    USERS {
        int id PK
        string email
        string password_hash
        string role "STUDENT, STAFF, MANAGER"
        int department_id FK
    }

    TICKETS {
        string ticket_id PK "VD: SUP-1001"
        string title
        text description
        string status "NEW, PROCESSING, PENDING, RESOLVED, CLOSED"
        int creator_id FK "SV tạo"
        int assignee_id FK "NV phụ trách"
        datetime created_at
        datetime resolved_at
    }

    AUDIT_LOGS {
        int log_id PK
        string ticket_id FK
        int action_by FK
        string old_status
        string new_status
        datetime created_at
    }
