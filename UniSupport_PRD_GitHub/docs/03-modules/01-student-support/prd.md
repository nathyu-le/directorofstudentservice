# PRD module: Cổng hỗ trợ sinh viên

Bản tổng hợp để review; file từng FR và requirements.json giữ cùng nội dung.

# [FR-STU-01] Gửi yêu cầu hỗ trợ

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Tạo yêu cầu có mã để trường tiếp nhận và sinh viên theo dõi.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| category_id | Bắt buộc | Loại vấn đề đang dùng; cho phép giá trị Chưa xác định để điều phối phân loại. |
| title | Bắt buộc | Văn bản không chỉ có khoảng trắng; tối đa CFG-01. |
| description | Bắt buộc | Nội dung vấn đề không rỗng; tối đa CFG-02. |
| submission_key | Hệ thống | Mã của lần gửi để nhận biết gửi lặp cùng thao tác. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên mở biểu mẫu và chọn loại vấn đề, nhập tiêu đề cùng mô tả.
2. Máy chủ lấy chủ yêu cầu từ phiên, kiểm tra trường và mã lần gửi.
3. Lưu yêu cầu, mã duy nhất, thời điểm tạo, trạng thái Đã tiếp nhận và sự kiện tiếp nhận trong cùng giao dịch.
4. Trả mã xác nhận và liên kết xem tiến độ; yêu cầu xuất hiện trong hàng chờ điều phối.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Thiếu nội dung: Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. → Báo tại trường; không tạo yêu cầu hay lịch sử một phần.
- Giả mạo chủ yêu cầu: Phiên SV-A gửi thêm student_id=SV-B. → Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.
- Gửi lặp: Gửi lại cùng submission_key hai lần. → Trả cùng mã; tổng số yêu cầu tăng một, không hai.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-01-01 | Đủ trường, loại hợp lệ | Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã. |
| AC-STU-01-02 | Thiếu nội dung | Báo tại trường; không tạo yêu cầu hay lịch sử một phần. |
| AC-STU-01-03 | Giả mạo chủ yêu cầu | Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt. |
| AC-STU-01-04 | Gửi lặp | Trả cùng mã; tổng số yêu cầu tăng một, không hai. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-01-01 | AC-STU-01-01 | Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục. |
| TC-STU-01-02 | AC-STU-01-02 | Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. |
| TC-STU-01-03 | AC-STU-01-03 | Phiên SV-A gửi thêm student_id=SV-B. |
| TC-STU-01-04 | AC-STU-01-04 | Gửi lại cùng submission_key hai lần. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.

Master dùng parent `FR-STU-01 | Gửi yêu cầu hỗ trợ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-IAM-03


---

# [FR-STU-02] Xem danh sách yêu cầu của tôi

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Tìm yêu cầu mình đã gửi và biết trạng thái hiện tại.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| keyword | Tùy chọn | Tìm mã hoặc tiêu đề; CFG-03. |
| status | Tùy chọn | Một trạng thái hợp lệ hoặc Tất cả. |
| page | Tùy chọn | Số nguyên dương; kích thước trang CFG-04. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên mở Yêu cầu của tôi.
2. Máy chủ giới hạn theo user_id của phiên rồi áp dụng tìm kiếm, lọc và phân trang.
3. Trả mã, tiêu đề, trạng thái, đơn vị/người phụ trách nếu đã có, thời điểm cập nhật; cho mở chi tiết.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Tham số sai: page=0 hoặc trạng thái không thuộc danh sách. → Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.
- Lọc theo người khác: SV-A sửa tham số student_id thành SV-B. → Không trả bất kỳ yêu cầu của SV-B.
- Danh sách rỗng: Tài khoản SV-C chưa có yêu cầu. → Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-02-01 | Có yêu cầu và bộ lọc | Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện. |
| AC-STU-02-02 | Tham số sai | Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền. |
| AC-STU-02-03 | Lọc theo người khác | Không trả bất kỳ yêu cầu của SV-B. |
| AC-STU-02-04 | Danh sách rỗng | Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-02-01 | AC-STU-02-01 | SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý. |
| TC-STU-02-02 | AC-STU-02-02 | page=0 hoặc trạng thái không thuộc danh sách. |
| TC-STU-02-03 | AC-STU-02-03 | SV-A sửa tham số student_id thành SV-B. |
| TC-STU-02-04 | AC-STU-02-04 | Tài khoản SV-C chưa có yêu cầu. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.

Master dùng parent `FR-STU-02 | Xem danh sách yêu cầu của tôi` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01


---

# [FR-STU-03] Xem chi tiết và tiến độ yêu cầu

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Biết yêu cầu đã được tiếp nhận, ai chịu trách nhiệm và có cần bổ sung thông tin.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Mã yêu cầu thuộc người dùng. |
| status / owner / due_at | Chỉ đọc | Lấy từ bản ghi hiện hành; hạn có thể chưa xác định. |
| public_history | Chỉ đọc | Các sự kiện và nội dung được công khai cho sinh viên. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên mở mã yêu cầu.
2. Máy chủ kiểm tra quyền trên yêu cầu trước khi đọc nội dung.
3. Hiển thị nội dung, trạng thái, trách nhiệm, hạn nếu có và lịch sử công khai.
4. Nếu Chờ bổ sung, hiển thị câu hỏi đang mở và thao tác bổ sung; nếu có kết quả, hiển thị kết quả và phản hồi.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Mã không tồn tại: Mã yêu cầu không có trong bộ dữ liệu. → Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.
- Truy cập ngang: SV-B mở URL yêu cầu của SV-A. → Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A.
- Chưa phân công / chờ bổ sung: Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. → Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-03-01 | Tiến độ đã cập nhật | Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại. |
| AC-STU-03-02 | Mã không tồn tại | Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu. |
| AC-STU-03-03 | Truy cập ngang | Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A. |
| AC-STU-03-04 | Chưa phân công / chờ bổ sung | Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-03-01 | AC-STU-03-01 | Yêu cầu được phân công rồi chuyển Đang xử lý. |
| TC-STU-03-02 | AC-STU-03-02 | Mã yêu cầu không có trong bộ dữ liệu. |
| TC-STU-03-03 | AC-STU-03-03 | SV-B mở URL yêu cầu của SV-A. |
| TC-STU-03-04 | AC-STU-03-04 | Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.

Master dùng parent `FR-STU-03 | Xem chi tiết và tiến độ yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-IAM-03


---

# [FR-STU-04] Bổ sung thông tin được yêu cầu

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Trả lời yêu cầu bổ sung trên cùng hồ sơ hỗ trợ.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| question_id | Bắt buộc | Câu hỏi đang mở thuộc yêu cầu. |
| content | Bắt buộc | Nội dung không rỗng; CFG-02. |
| record_version | Hệ thống | Phiên bản dùng khi gửi để phát hiện dữ liệu đã đổi. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên đọc câu hỏi và nhập nội dung trả lời.
2. Máy chủ kiểm tra chủ hồ sơ, trạng thái, câu hỏi và phiên bản.
3. Lưu câu trả lời vào cùng request_id, đánh dấu câu hỏi đã được trả lời và chuyển về Đang xử lý.
4. Giữ người phụ trách; thêm lịch sử công khai để bên xử lý tiếp tục.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Nội dung trống: Trả lời chỉ có khoảng trắng. → Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.
- Sai chủ: SV-B gửi câu trả lời cho yêu cầu SV-A. → Từ chối; nội dung và trạng thái không đổi.
- Gửi từ màn hình cũ: Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. → Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-04-01 | Trả lời hợp lệ | Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý. |
| AC-STU-04-02 | Nội dung trống | Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời. |
| AC-STU-04-03 | Sai chủ | Từ chối; nội dung và trạng thái không đổi. |
| AC-STU-04-04 | Gửi từ màn hình cũ | Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-04-01 | AC-STU-04-01 | Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01. |
| TC-STU-04-02 | AC-STU-04-02 | Trả lời chỉ có khoảng trắng. |
| TC-STU-04-03 | AC-STU-04-03 | SV-B gửi câu trả lời cho yêu cầu SV-A. |
| TC-STU-04-04 | AC-STU-04-04 | Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.

Master dùng parent `FR-STU-04 | Bổ sung thông tin được yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-07, FR-STU-03


---

# [FR-STU-05] Phản hồi kết quả hỗ trợ

**Module:** Cổng hỗ trợ sinh viên

**Nguồn phạm vi:** Proposal §2.1 — gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ, phản hồi kết quả. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Ghi nhận phản hồi và điểm hài lòng về kết quả.

## Actor

Sinh viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| score | Bắt buộc | Số nguyên 1–5 theo CFG-05; có nhãn ý nghĩa. |
| comment | Tùy chọn | Nhận xét tối đa CFG-02. |
| result_id | Hệ thống | Kết quả hiện hành của yêu cầu. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Sinh viên đọc kết quả, chọn điểm và nhập nhận xét nếu có.
2. Máy chủ kiểm tra chủ hồ sơ, kết quả và điểm.
3. Lưu một phản hồi hiện hành cho yêu cầu; gửi lại cập nhật phản hồi đó.
4. Hiển thị phản hồi đã lưu; quản lý đọc qua báo cáo hài lòng. Không tự mở lại yêu cầu.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Điểm sai / chưa có kết quả: Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. → Từ chối; không có phản hồi mới để tính báo cáo.
- Không phải chủ: SV-B gửi đánh giá cho yêu cầu SV-A. → Từ chối; phản hồi của SV-A không bị sửa.
- Cập nhật phản hồi: SV-A đổi điểm từ 4 thành 3. → Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-STU-05-01 | Phản hồi hợp lệ | Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4. |
| AC-STU-05-02 | Điểm sai / chưa có kết quả | Từ chối; không có phản hồi mới để tính báo cáo. |
| AC-STU-05-03 | Không phải chủ | Từ chối; phản hồi của SV-A không bị sửa. |
| AC-STU-05-04 | Cập nhật phản hồi | Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-STU-05-01 | AC-STU-05-01 | Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn. |
| TC-STU-05-02 | AC-STU-05-02 | Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. |
| TC-STU-05-03 | AC-STU-05-03 | SV-B gửi đánh giá cho yêu cầu SV-A. |
| TC-STU-05-04 | AC-STU-05-04 | SV-A đổi điểm từ 4 thành 3. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.

Master dùng parent `FR-STU-05 | Phản hồi kết quả hỗ trợ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-09
