# Hợp đồng API v5.0

Thiết kế đề xuất REST/JSON trong prototype. POST JSONUTF8+CSRF header; GET không thay dữ liệu; server lấy actor từ session.

| FR | Method | Path | Input | Output |
| --- | --- | --- | --- | --- |
| FR-STU-01 | POST | /api/requests | category_id, title, description, submission_key | 201/200:request_id, reference, status, record_version |
| FR-STU-02 | GET | /api/student/requests | keyword, status, page | 200:items, total, page, page_size |
| FR-STU-03 | GET | /api/student/requests/{id} | id | 200:detail, public_history, open_question, result, feedback |
| FR-STU-04 | POST | /api/student/requests/{id}/answers | question_id, content, record_version | 200:status, record_version, answer_id |
| FR-STU-05 | POST | /api/student/requests/{id}/feedback | result_id, score, comment, record_version | 200:feedback_id, score, updated_at, record_version |
| FR-DSP-01 | GET | /api/dispatch/requests | keyword, category_id, status, department_id, unassigned, page | 200:items, total, page, page_size |
| FR-DSP-02 | GET | /api/staff/requests/{id} | id | 200:detail, history, question, result, record_version |
| FR-DSP-03 | POST | /api/dispatch/requests/{id}/category | category_id, reason, record_version | 200:category_id, record_version, no_change |
| FR-DSP-04 | POST | /api/dispatch/requests/{id}/assignment | department_id, assignee_id, reason, record_version | 200:department_id, assignee_id, record_version, no_change |
| FR-DSP-05 | POST | /api/dispatch/requests/{id}/due | due_at, reason, record_version | 200:due_at, record_version, no_change |
| FR-DSP-06 | POST | /api/staff/requests/{id}/start | record_version | 200:status, started_at, record_version |
| FR-DSP-07 | POST | /api/staff/requests/{id}/questions | question, record_version | 200:question_id, status, record_version |
| FR-DSP-08 | POST | /api/staff/requests/{id}/progress | content, visibility, record_version | 200:note_id, record_version |
| FR-DSP-09 | POST | /api/staff/requests/{id}/resolve | result_content, record_version | 200:result_id, status, resolved_at, record_version |
| FR-DSP-10 | POST | /api/dispatch/requests/{id}/close | record_version | 200:status, closed_at, record_version |
| FR-RPT-01 | GET | /api/reports/01 | department_id, category_id, from_date, to_date | 200:filters, as_of, sample_count, metrics, source_references |
| FR-RPT-02 | GET | /api/reports/02 | department_id, category_id | 200:filters, as_of, sample_count, metrics, source_references |
| FR-RPT-03 | GET | /api/reports/03 | department_id, category_id, from_date, to_date | 200:filters, as_of, sample_count, metrics, source_references |
| FR-RPT-04 | GET | /api/reports/04 | department_id, category_id, from_date, to_date | 200:filters, as_of, sample_count, metrics, source_references |
| FR-RPT-05 | GET | /api/reports/05 | department_id, category_id | 200:filters, as_of, sample_count, metrics, source_references |
| FR-RPT-06 | GET | /api/reports/06 | department_id, category_id, from_date, to_date | 200:filters, as_of, sample_count, metrics, source_references |
| FR-IAM-01 | POST | /api/auth/login | email, password | 200:user_id, display_name, capabilities, csrf_token |
| FR-IAM-02 | POST | /api/auth/logout | CSRF khi có phiên | 204:no body |
| FR-IAM-03 | Middleware | Tất cả protected endpoints | session/capability/scope/object/CSRF | Không endpoint mới; allow/deny trước query |

Success envelope JSON {data:...}; error envelope {error:{code, message, fields:{field:message}}, correlation_id}. 204 không body. Bộ lọc không hỗ trợ 422; role/scope/student_idclient bị bỏ qua không nâng quyền. Path id không phải integer dương 422; ID hợp lệ ngoài scope 404. DateISO8601; enum dùng mã 5trạng thái. Report filters phản ánh đúng đã áp dụng và as_of một giá trị UTC.

CREATED history public; CATEGORY_CHANGED public(type cũ/mới); ASSIGNED public(đơn vị/người cũ/mới, lý do phối hợp); DUE_CHANGED public; STARTEDpublic; INFO_REQUESTED/INFO_ANSWEREDpublic; PROGRESS theo visibility; RESOLVED/CLOSEDpublic; FEEDBACK_UPDATEDpublic(chủ SV xem feedback của mình). Error không stacktrace/SQL/password. Internal history không bị trộn vào public projection.

Đồng thời: SELECT request FOR UPDATE, đọc quyền/trạng thái/version lại trongtransaction, validate invariants, ghi tất cả, commit. Idempotency create unique constraint phát hiện race và đọc submission đã commit để replay, không trả generic 500 khi unique violationđãbiết. Conflict versionnêu version hiện hành tùy quyền nhưng không ghi đè. Nếu 500 trước commit rollback; response mất saucommit xử lý theo FR.
