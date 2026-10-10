### [FR-STU-03] Xem chi tiết và tiến độ yêu cầu

**Mô tả**

Biết yêu cầu đã được tiếp nhận, ai chịu trách nhiệm và có cần bổ sung thông tin. Đặc tả triển khai prototype thuộc Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả; các chi tiết trường và quy tắc dưới đây là thiết kế đề xuất v5.0 để review, không phải thông tin vận hành đã được khách hàng xác nhận.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Preconditions**

- Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.
- Seed là dữ liệu giả lập. Trước triển khai, BE/FE/QA review các quyết định liên quan trong danh sách câu hỏi mở; không coi bản dự thảo là đã được duyệt.

**Luồng chính**

1. Sinh viên mở mã yêu cầu.
2. Máy chủ kiểm tra quyền trên yêu cầu trước khi đọc nội dung.
3. Hiển thị nội dung, trạng thái, trách nhiệm, hạn nếu có và lịch sử công khai.
4. Nếu Chờ bổ sung, hiển thị câu hỏi đang mở và thao tác bổ sung; nếu có kết quả, hiển thị kết quả và phản hồi.

**Business Rules**

| Thông tin | Bắt buộc/nguồn | Quy định |
| --- | --- | --- |
| request_id | Bắt buộc | Mã yêu cầu thuộc người dùng. |
| status / owner / due_at | Chỉ đọc | Lấy từ bản ghi hiện hành; hạn có thể chưa xác định. |
| public_history | Chỉ đọc | Các sự kiện và nội dung được công khai cho sinh viên. |

- request_id là số nguyên dương. Chỉ chủ sinh viên xem được. Không tồn tại hoặc khác chủ trả cùng HTTP 404 REQUEST_NOT_FOUND.
- Hiển thị title, description, category, status, đơn vị/người phụ trách, hạn, kết quả, phản hồi hiện hành và câu hỏi bổ sung đang mở. Không có câu hỏi mở thì không hiển thị nút trả lời.
- public_history chỉ gồm visibility=public, sắp occurred_at tăng dần rồi id tăng dần. Máy chủ loại bỏ nội dung internal trước serialize JSON, không chỉ ẩn bằng CSS.
- Nút bổ sung chỉ khi WaitingInfo và có câu hỏi mở. Nút phản hồi chỉ khi Resolved/Closed và có result_id. Các nút không cấp quyền thay máy chủ. Sau tải lại dùng dữ liệu hiện hành, không snapshot cũ.

**Alternative / Error Flows**

- 401: phiên không hợp lệ; điều hướng đăng nhập, không gửi lại thao tác ghi tự động sau login.
- 403: thiếu capability/CSRF hoặc bộ lọc ngoài scope; không trả dữ liệu trái quyền.
- 404: request không tồn tại/ngoài quyền đối tượng; không tiết lộ hồ sơ có tồn tại hay không.
- 422: sai trường/bộ lọc theo Business Rules; hiển thị lỗi tại trường, giữ dữ liệu đang nhập.
- 500 hoặc mất mạng: không đánh thao tác thành công khi chưa có response xác nhận. Riêng STU-01 retry dùng cùng submission_key; các thao tác khác tải lại để xác định lần ghi đã commit, không gửi tự động vô điều kiện.

**Acceptance Criteria**

- **AC-STU-03-01:** Khi Yêu cầu được phân công rồi chuyển Đang xử lý. → Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.
- **AC-STU-03-02:** Khi Mã yêu cầu không có trong bộ dữ liệu. → Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.
- **AC-STU-03-03:** Khi SV-B mở URL yêu cầu của SV-A. → Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A.
- **AC-STU-03-04:** Khi Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. → Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.
- **AC-STU-03-05:** Khi Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp. → JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history.

**Ví dụ Edge Case**

Không lộ internal JSON: Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp.

**Expected Result**

JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history. Kiểm theo TC-STU-03-05; kết quả thực thi ban đầu là Not Run.
