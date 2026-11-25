### [FR-STU-03] Xem chi tiết và tiến độ yêu cầu

**Mô tả**

Biết yêu cầu đã được tiếp nhận, ai chịu trách nhiệm và có cần bổ sung thông tin. Phạm vi và mục tiêu xem [tổng quan sản phẩm](../../../01-overview/scope.md). Các chi tiết nghiệp vụ đang ở trạng thái đề xuất.

**Actor**

Sinh viên. Quyền cụ thể kiểm ở máy chủ theo [ma trận quyền](../../../02-domain/permissions.md).

**Priority**

P0 theo [thang ưu tiên PRD](../../../01-overview/scope.md#ưu-tiên-yêu-cầu).

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

- request_id là số nguyên dương. Chỉ chủ sinh viên xem được. Không tồn tại hoặc khác chủ trả cùng HTTP 404 REQUEST_NOT_FOUND. **Kiểm bởi:** AC-STU-03-02, AC-STU-03-03, AC-STU-03-07.
- Hiển thị title, description, category, status, đơn vị/người phụ trách, hạn, kết quả, phản hồi hiện hành và câu hỏi bổ sung đang mở. Không có câu hỏi mở thì không hiển thị nút trả lời. **Kiểm bởi:** AC-STU-03-01, AC-STU-03-04, AC-STU-03-06.
- public_history chỉ gồm visibility=public, sắp occurred_at tăng dần rồi id tăng dần. Máy chủ loại bỏ nội dung internal trước serialize JSON, không chỉ ẩn bằng CSS. **Kiểm bởi:** AC-STU-03-05, AC-STU-03-06.
- Nút bổ sung chỉ khi WaitingInfo và có câu hỏi mở. Nút phản hồi chỉ khi Resolved/Closed và có result_id. Các nút không cấp quyền thay máy chủ. Sau tải lại dùng dữ liệu hiện hành, không snapshot cũ. **Kiểm bởi:** AC-STU-03-01, AC-STU-03-06.

**Alternative / Error Flows**

- 401 khi phiên hết hiệu lực. Xem NFR-SEC-01 về xác thực lại và xử lý thao tác đang chờ.
- 403 thiếu quyền dùng cổng sinh viên. **Kiểm bởi:** NFR-SEC-01.
- 404 mã không có hoặc ngoài quyền đối tượng. **Kiểm bởi:** AC-STU-03-02, AC-STU-03-03.
- 422 ID sai kiểu. **Kiểm bởi:** AC-STU-03-07.
- Xử lý mất kết nối, thông báo lỗi biểu mẫu và mã đối chiếu theo [NFR-OBS-01](../../../01-overview/non-functional-requirements.md).

**Acceptance Criteria**

- **AC-STU-03-01:** Khi Yêu cầu được phân công rồi chuyển Đang xử lý. → Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.
- **AC-STU-03-02:** Khi Mã yêu cầu không có trong bộ dữ liệu. → Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.
- **AC-STU-03-03:** Khi SV-B mở URL yêu cầu của SV-A. → Không trả tiêu đề, mô tả, lịch sử hoặc dữ liệu kèm theo của SV-A.
- **AC-STU-03-04:** Khi Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. → Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.
- **AC-STU-03-05:** Khi Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp. → JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history.

- **AC-STU-03-06:** Khi Mở hồ sơ mới, chờ bổ sung có câu hỏi mở, đã giải quyết, rồi hồ sơ đã đóng. Thêm hai sự kiện công khai cùng giờ và một sự kiện nội bộ. → Chỉ hiển thị nút trả lời khi có câu hỏi mở trong WaitingInfo, nút phản hồi khi Resolved/Closed có kết quả. Lịch sử công khai theo giờ rồi ID tăng và không có nội bộ.
- **AC-STU-03-07:** Khi Mở URL chi tiết với ID 0, chữ và ID hợp lệ nhưng không tồn tại. → ID sai định dạng trả 422. ID không có trả 404 với nội dung giống trường hợp ngoài quyền, không trả dữ liệu nghiệp vụ.

**Ví dụ Edge Case**

Không lộ internal JSON: Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp.

**Expected Result**

JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history. Kiểm theo TC-STU-03-05; kết quả thực thi ban đầu là Not Run.

