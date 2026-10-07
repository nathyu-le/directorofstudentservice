# ADR-004 Kiểm soát nhận Ticket đồng thời

**Trạng thái:** Đề nghị cho review kỹ thuật.

## Bối cảnh

NV nhận hàng chờ có thể đồng thời với NV khác hoặc phân công của QL.

## Quyết định

Dùng một transaction cập nhật có điều kiện với ticket_id, version, status RECEIVED, assignee_id null và department_id đúng; chỉ thành công khi một hàng được đổi. Khóa hoặc điều kiện nguyên tử được dùng cùng kiểm danh mục/tài khoản để loại việc nhận bằng dữ liệu cũ.

## Hệ quả và kiểm chứng

Chỉ giao dịch thắng ghi assignee, PROCESSING, started_at, event và result. Giao dịch thua trả xung đột, không có event nghiệp vụ. QL phân công dùng cùng điều kiện chưa phân công. Test đồng thời dùng hai connection thật, không chỉ bấm tuần tự.

## Yêu cầu liên quan

FR-OPS-01, FR-ASG-01; BR-02, BR-09; E2E-03, NFR-REL-01.

[Về danh mục PRD](../../README.md)
