# [FR-IAM-02] Đăng xuất

**Module:** Tài khoản và quyền truy cập

**Nguồn phạm vi:** Proposal §2.1 — xác thực, phân quyền, hạn chế truy cập thông tin và tài liệu liên quan. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Kết thúc phiên sử dụng hiện tại.

## Actor

Mọi vai trò đã đăng nhập. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đã đăng nhập hợp lệ.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| session | Hệ thống | Phiên hiện tại trên máy chủ. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Người dùng chọn Đăng xuất.
2. Máy chủ vô hiệu phiên hiện tại và xóa thông tin phiên phía trình duyệt.
3. Chuyển về đăng nhập; mọi thao tác với phiên cũ phải bị chặn.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Không có phiên: Gọi đăng xuất khi đã thoát. → Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.
- Dùng lại phiên: Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. → Bị chặn; dữ liệu không thay đổi.
- Back / đăng nhập lại: Back trang trước rồi đăng nhập lại. → Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-IAM-02-01 | Đăng xuất đúng | Trở về đăng nhập; phiên máy chủ bị vô hiệu. |
| AC-IAM-02-02 | Không có phiên | Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác. |
| AC-IAM-02-03 | Dùng lại phiên | Bị chặn; dữ liệu không thay đổi. |
| AC-IAM-02-04 | Back / đăng nhập lại | Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-IAM-02-01 | AC-IAM-02-01 | Đăng nhập rồi chọn Đăng xuất. |
| TC-IAM-02-02 | AC-IAM-02-02 | Gọi đăng xuất khi đã thoát. |
| TC-IAM-02-03 | AC-IAM-02-03 | Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. |
| TC-IAM-02-04 | AC-IAM-02-04 | Back trang trước rồi đăng nhập lại. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Trở về đăng nhập; phiên máy chủ bị vô hiệu.

Master dùng parent `FR-IAM-02 | Đăng xuất` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-IAM-01
