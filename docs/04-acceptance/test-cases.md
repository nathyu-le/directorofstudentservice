# Test case chức năng

Dùng [fixture và quy trình chạy](fixtures.md), [hợp đồng API](../02-domain/api-contract.md) và [ma trận truy vết](traceability-matrix.md). Mọi kết quả là Not Run. Lưu kết quả thực tế và bằng chứng riêng theo phiếu chạy QC.

## TC-STU-01-01 — Đủ trường, loại hợp lệ

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.

**Kết quả:** Not Run.

## TC-STU-01-02 — Thiếu nội dung

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Tiêu đề chỉ có khoảng trắng hoặc mô tả trống. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Báo tại trường; không tạo yêu cầu hay lịch sử một phần.

**Kết quả:** Not Run.

## TC-STU-01-03 — Giả mạo chủ yêu cầu

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Phiên SV-A gửi thêm student_id=SV-B. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.

**Kết quả:** Not Run.

## TC-STU-01-04 — Gửi lặp

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Gửi lại cùng submission_key hai lần. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Trả cùng mã; tổng số yêu cầu tăng một, không hai.

**Kết quả:** Not Run.

## TC-STU-01-05 — Mất response rồi retry

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Commit tạo xong nhưng client không nhận response; gửi lại cùng khóa/payload. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** HTTP 200 cùng reference; DB 1 request và 1 event CREATED cho khóa.

**Kết quả:** Not Run.

## TC-STU-01-06 — Khóa bị tái sử dụng sai

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Cùng student/key nhưng đổi description. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 409 SUBMISSION_KEY_CONFLICT; bản đã tạo không đổi.

**Kết quả:** Not Run.

## TC-STU-01-07 — Hai ý định riêng

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Cùng nội dung nhưng hai UUID khác nhau. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Tạo 2 yêu cầu hợp lệ; không chống trùng theo nội dung để làm mất ý định mới.

**Kết quả:** Not Run.

## TC-STU-01-08 — Rollback giao dịch

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-08

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Đọc và lưu ảnh chụp submissions, requests và history theo khóa hoặc request_id trước thao tác. Dùng payload hợp lệ của actor SV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/requests` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so submissions, requests và history theo khóa hoặc request_id với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-STU-01-09 — Biên nhập và danh mục

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-09

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Gửi riêng tiêu đề Unicode dài 150 và 151 ký tự, mô tả dài 4000 và 4001 ký tự, category 199 đã ngừng và category 103 Chưa xác định. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Chỉ chấp nhận khi không quá giới hạn và danh mục hoạt động. Báo lỗi tại trường sai, giữ dữ liệu nhập và không thêm hồ sơ/lịch sử ở lượt bị từ chối.

**Kết quả:** Not Run.

## TC-STU-01-10 — Trường do hệ thống sở hữu

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-10

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. SV-A gửi UUID hợp lệ kèm student_id=102, role=Manager, department_id=201, assignee_id=111, status=Closed và các mốc do máy khách chỉ định. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Máy chủ lấy chủ từ phiên, tạo Received, phiên bản 1, trường trách nhiệm và hạn/mốc xử lý rỗng, giờ tạo từ máy chủ. Có đúng một submission và một CREATED công khai.

**Kết quả:** Not Run.

## TC-STU-01-11 — Định dạng khóa và chuẩn hóa

**Yêu cầu kiểm:** FR-STU-01 · AC-STU-01-11

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo yêu cầu mới theo JSON cơ sở trong fixture.

1. Gửi submission_key không đúng UUID v4. Sau đó gửi khóa hợp lệ với khoảng trắng đầu/cuối, rồi gửi lại cùng khóa với chuỗi đã bỏ khoảng trắng. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/requests` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm submissions, requests và history theo khóa hoặc request_id theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Khóa sai báo 422. Hai cách nhập tương đương trả 201 rồi 200 cùng mã, chỉ một submission/hồ sơ/CREATED.

**Kết quả:** Not Run.

## TC-STU-02-01 — Có yêu cầu và bộ lọc

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.

**Kết quả:** Not Run.

## TC-STU-02-02 — Tham số sai

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. page=0 hoặc trạng thái không thuộc danh sách. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.

**Kết quả:** Not Run.

## TC-STU-02-03 — Lọc theo người khác

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. SV-A sửa tham số student_id thành SV-B. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Không trả bất kỳ yêu cầu của SV-B.

**Kết quả:** Not Run.

## TC-STU-02-04 — Danh sách rỗng

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. Tài khoản SV-C chưa có yêu cầu. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.

**Kết quả:** Not Run.

## TC-STU-02-05 — Trang vượt cuối

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. Có 1 request, page=2 với page_size 20. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** items=[] và total=1; không trả request người khác, không tự sửa page.

**Kết quả:** Not Run.

## TC-STU-02-06 — Tìm theo mã hoặc tiêu đề

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. Với SV-A, tìm từ khóa có khoảng trắng đầu/cuối và khác chữ hoa/thường, khớp một phần mã US-310 hoặc tiêu đề Học vụ. Thử từ khóa 100 và 101 ký tự. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Tìm trên mã/tiêu đề đúng phạm vi. 100 ký tự hợp lệ, 101 trả 422 tại bộ lọc. Không đưa hồ sơ SV-B vào items/total.

**Kết quả:** Not Run.

## TC-STU-02-07 — Thứ tự, phân trang và nhãn rỗng

**Yêu cầu kiểm:** FR-STU-02 · AC-STU-02-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng hồ sơ 310, 315 và đối chứng 316. Với tập rỗng dùng SV-C.

1. Tạo 21 hồ sơ của SV-A. Hai hồ sơ mới nhất trùng updated_at nhưng ID khác. Một hồ sơ không có người/hạn. Đọc trang 1 và 2. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests` với các biến thể đã nêu. So JSON items/total và thứ tự các request_id trong bộ lọc trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** 20 dòng rồi 1 dòng, cùng total=21, thứ tự updated_at và ID giảm dần. Hiện Chưa phân công, Chưa đặt hạn. Không có email riêng nhân viên trong JSON.

**Kết quả:** Not Run.

## TC-STU-03-01 — Tiến độ đã cập nhật

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. Yêu cầu được phân công rồi chuyển Đang xử lý. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.

**Kết quả:** Not Run.

## TC-STU-03-02 — Mã không tồn tại

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. Mã yêu cầu không có trong bộ dữ liệu. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.

**Kết quả:** Not Run.

## TC-STU-03-03 — Truy cập ngang

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-B. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. SV-B mở URL yêu cầu của SV-A. Mở màn hình tương ứng bằng SV-B, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Không trả tiêu đề, mô tả, lịch sử hoặc dữ liệu kèm theo của SV-A.

**Kết quả:** Not Run.

## TC-STU-03-04 — Chưa phân công / chờ bổ sung

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.

**Kết quả:** Not Run.

## TC-STU-03-05 — Không lộ internal JSON

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history.

**Kết quả:** Not Run.

## TC-STU-03-06 — Lịch sử và hành động đúng trạng thái

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. Mở hồ sơ mới, chờ bổ sung có câu hỏi mở, đã giải quyết, rồi hồ sơ đã đóng. Thêm hai sự kiện công khai cùng giờ và một sự kiện nội bộ. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Chỉ hiển thị nút trả lời khi có câu hỏi mở trong WaitingInfo, nút phản hồi khi Resolved/Closed có kết quả. Lịch sử công khai theo giờ rồi ID tăng và không có nội bộ.

**Kết quả:** Not Run.

## TC-STU-03-07 — Mã không hợp lệ

**Yêu cầu kiểm:** FR-STU-03 · AC-STU-03-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Đọc 310, 311, 313 hoặc 315 theo trạng thái của tình huống.

1. Mở URL chi tiết với ID 0, chữ và ID hợp lệ nhưng không tồn tại. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/student/requests/{id}` với các biến thể đã nêu. So JSON chi tiết, lịch sử công khai và nút trên giao diện trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** ID sai định dạng trả 422. ID không có trả 404 với nội dung giống trường hợp ngoài quyền, không trả dữ liệu nghiệp vụ.

**Kết quả:** Not Run.

## TC-STU-04-01 — Trả lời hợp lệ

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.

**Kết quả:** Not Run.

## TC-STU-04-02 — Nội dung trống

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Trả lời chỉ có khoảng trắng. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.

**Kết quả:** Not Run.

## TC-STU-04-03 — Sai chủ

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-B. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. SV-B gửi câu trả lời cho yêu cầu SV-A. Dùng SV-B hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; nội dung và trạng thái không đổi.

**Kết quả:** Not Run.

## TC-STU-04-04 — Gửi từ màn hình cũ

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.

**Kết quả:** Not Run.

## TC-STU-04-05 — Tiếp tục không reset mốc

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Ghi lại started_at, due_at, owner trước khi trả lời question mở. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Sau bổ sung cả 3 giá trị giữ nguyên, Processing, một câu trả lời cho question.

**Kết quả:** Not Run.

## TC-STU-04-06 — Rollback giao dịch

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Đọc và lưu ảnh chụp requests, questions, answers và history theo 311/401 trước thao tác. Dùng payload hợp lệ của actor SV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/student/requests/{id}/answers` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests, questions, answers và history theo 311/401 với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-STU-04-07 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Đọc phiên bản trong requests, questions, answers và history theo 311/401, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/student/requests/{id}/answers` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests, questions, answers và history theo 311/401 đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-STU-04-08 — Câu hỏi của hồ sơ khác

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-08

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. SV-A lấy câu hỏi 402 thuộc hồ sơ 312 và gửi cho hồ sơ 311 đang WaitingInfo, kèm phiên bản đúng của 311. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 422 QUESTION_INVALID. Giữ nguyên status, version và câu hỏi của hai hồ sơ. Không tạo answer hoặc history.

**Kết quả:** Not Run.

## TC-STU-04-09 — Biên nội dung và phiên bản

**Yêu cầu kiểm:** FR-STU-04 · AC-STU-04-09

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 311, question_id=401, content “Mã lớp A01” và version hiện hành.

1. Dùng câu hỏi 401 của hồ sơ 311. Gửi nội dung 4001 ký tự, record_version=0 rồi nội dung 4000 ký tự với phiên bản hiện hành. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/answers` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions, answers và history theo 311/401 theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Hai lượt sai báo 422 và giữ đầu vào. Lượt hợp lệ ghi đúng một answer, đóng câu hỏi, tăng version và ghi INFO_ANSWERED công khai.

**Kết quả:** Not Run.

## TC-STU-05-01 — Phản hồi hợp lệ

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.

**Kết quả:** Not Run.

## TC-STU-05-02 — Điểm sai / chưa có kết quả

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không có phản hồi mới để tính báo cáo.

**Kết quả:** Not Run.

## TC-STU-05-03 — Không phải chủ

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-B. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. SV-B gửi đánh giá cho yêu cầu SV-A. Dùng SV-B hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; phản hồi của SV-A không bị sửa.

**Kết quả:** Not Run.

## TC-STU-05-04 — Cập nhật phản hồi

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. SV-A đổi điểm từ 4 thành 3. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.

**Kết quả:** Not Run.

## TC-STU-05-05 — Phản hồi khi đã đóng

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Chủ SV gửi score 5 trên Closed có result. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lưu feedback, status vẫn Closed, closed_at không đổi.

**Kết quả:** Not Run.

## TC-STU-05-06 — Rollback giao dịch

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Đọc và lưu ảnh chụp requests, results, feedback và history theo hồ sơ trước thao tác. Dùng payload hợp lệ của actor SV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/student/requests/{id}/feedback` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests, results, feedback và history theo hồ sơ với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-STU-05-07 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Đọc phiên bản trong requests, results, feedback và history theo hồ sơ, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/student/requests/{id}/feedback` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests, results, feedback và history theo hồ sơ đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-STU-05-08 — Độ dài nhận xét và kết quả hiện hành

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-08

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Trên hồ sơ 313 gửi nhận xét có khoảng trắng đầu/cuối với 4000 rồi 4001 ký tự sau bỏ khoảng trắng. Thử result_id của hồ sơ khác và version hiện hành. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Chấp nhận nhận xét hợp lệ, báo trường sai cho nhận xét 4001, từ chối result_id sai. Giữ kết quả, trạng thái và phản hồi cũ ở các lượt lỗi.

**Kết quả:** Not Run.

## TC-STU-05-09 — Cập nhật thời điểm và báo cáo

**Yêu cầu kiểm:** FR-STU-05 · AC-STU-05-09

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 313, result_id=501, score=4, comment “Đã được hướng dẫn” và version hiện hành. Case Closed dùng 314/502.

1. Thêm phản hồi 4 điểm trên hồ sơ 313, đổi thành 3 ở ngày hôm sau. Đọc nguồn của RPT-06 cho hai ngày. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/student/requests/{id}/feedback` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results, feedback và history theo hồ sơ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Giữ feedback_id và created_at, cập nhật updated_at, không sinh dòng phản hồi thứ hai. Hồ sơ và kết quả giữ nguyên; báo cáo dùng kỳ cập nhật mới.

**Kết quả:** Not Run.

## TC-DSP-01-01 — Yêu cầu mới

**Yêu cầu kiểm:** FR-DSP-01 · AC-DSP-01-01

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Nạp 310 chưa giao, 315 đã giao và 316 thuộc phòng B.

1. Tạo yêu cầu Chưa xác định bằng STU-01. Mở màn hình tương ứng bằng DP-All, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/dispatch/requests` với các biến thể đã nêu. So items/total theo quyền, đối chiếu requests trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.

**Kết quả:** Not Run.

## TC-DSP-01-02 — Lọc sai

**Yêu cầu kiểm:** FR-DSP-01 · AC-DSP-01-02

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Nạp 310 chưa giao, 315 đã giao và 316 thuộc phòng B.

1. category_id không tồn tại. Mở màn hình tương ứng bằng DP-All, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/dispatch/requests` với các biến thể đã nêu. So items/total theo quyền, đối chiếu requests trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Báo tham số sai; không áp dụng loại khác âm thầm.

**Kết quả:** Not Run.

## TC-DSP-01-03 — Vai trò không được phép

**Yêu cầu kiểm:** FR-DSP-01 · AC-DSP-01-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp 310 chưa giao, 315 đã giao và 316 thuộc phòng B.

1. Sinh viên gọi trang/điểm truy cập hàng chờ. Mở màn hình tương ứng bằng SV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/dispatch/requests` với các biến thể đã nêu. So items/total theo quyền, đối chiếu requests trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Từ chối; không lộ danh sách toàn trường.

**Kết quả:** Not Run.

## TC-DSP-01-04 — Sau phân công

**Yêu cầu kiểm:** FR-DSP-01 · AC-DSP-01-04

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Nạp 310 chưa giao, 315 đã giao và 316 thuộc phòng B.

1. Phân công yêu cầu rồi lọc unassigned=true. Mở màn hình tương ứng bằng DP-All, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/dispatch/requests` với các biến thể đã nêu. So items/total theo quyền, đối chiếu requests trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.

**Kết quả:** Not Run.

## TC-DSP-01-05 — Tìm kiếm, thứ tự và phân trang

**Yêu cầu kiểm:** FR-DSP-01 · AC-DSP-01-05

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Nạp 310 chưa giao, 315 đã giao và 316 thuộc phòng B.

1. DP-All mở hàng chờ mặc định có hồ sơ mở/đóng, hai hồ sơ mở trùng created_at và 21 hồ sơ mở. Tìm theo phần mã hoặc tiêu đề rồi thử từ khóa 101 ký tự. Mở màn hình tương ứng bằng DP-All, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/dispatch/requests` với các biến thể đã nêu. So items/total theo quyền, đối chiếu requests trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Mặc định chỉ mở, có cả chưa giao; sắp created_at/ID tăng. Trang tối đa 20 và total thống nhất. Tìm đúng tập nguồn; báo 422 khi từ khóa quá dài.

**Kết quả:** Not Run.

## TC-DSP-01-06 — Điều phối trong phạm vi

**Yêu cầu kiểm:** FR-DSP-01 · AC-DSP-01-06

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Nạp 310 chưa giao, 315 đã giao và 316 thuộc phòng B.

1. DP-A thử lọc PB-B và đọc hàng chờ mặc định. DP-All xem cùng dữ liệu. Thử page=0, unassigned sai kiểu, status không hợp lệ và department_id=0. Mở màn hình tương ứng bằng DP-All, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/dispatch/requests` với các biến thể đã nêu. So items/total theo quyền, đối chiếu requests trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** DP-A không thấy hồ sơ ngoài PB-A hoặc chưa giao, lọc PB-B trả 403. DP-All thấy hồ sơ chưa giao. Các bộ lọc không hợp lệ trả 422.

**Kết quả:** Not Run.

## TC-DSP-02-01 — Có lịch sử

**Yêu cầu kiểm:** FR-DSP-02 · AC-DSP-02-01

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, thêm lịch sử CREATED, CATEGORY_CHANGED và ASSIGNED.

1. Yêu cầu đã tiếp nhận, phân loại, phân công. Mở màn hình tương ứng bằng NV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/staff/requests/{id}` với các biến thể đã nêu. So JSON history và các trường trên hồ sơ 315 trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.

**Kết quả:** Not Run.

## TC-DSP-02-02 — Mã không có

**Yêu cầu kiểm:** FR-DSP-02 · AC-DSP-02-02

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, thêm lịch sử CREATED, CATEGORY_CHANGED và ASSIGNED.

1. Mở mã không tồn tại. Mở màn hình tương ứng bằng NV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/staff/requests/{id}` với các biến thể đã nêu. So JSON history và các trường trên hồ sơ 315 trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Không tìm thấy; không sinh dữ liệu.

**Kết quả:** Not Run.

## TC-DSP-02-03 — Ngoài phạm vi

**Yêu cầu kiểm:** FR-DSP-02 · AC-DSP-02-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-B. Dùng 315, thêm lịch sử CREATED, CATEGORY_CHANGED và ASSIGNED.

1. Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. Mở màn hình tương ứng bằng NV-B, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/staff/requests/{id}` với các biến thể đã nêu. So JSON history và các trường trên hồ sơ 315 trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Từ chối, không trả nội dung sinh viên.

**Kết quả:** Not Run.

## TC-DSP-02-04 — Ghi chú nội bộ

**Yêu cầu kiểm:** FR-DSP-02 · AC-DSP-02-04

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, thêm lịch sử CREATED, CATEGORY_CHANGED và ASSIGNED.

1. Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. Mở màn hình tương ứng bằng NV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/staff/requests/{id}` với các biến thể đã nêu. So JSON history và các trường trên hồ sơ 315 trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.

**Kết quả:** Not Run.

## TC-DSP-02-05 — Phạm vi quản lý và thứ tự lịch sử

**Yêu cầu kiểm:** FR-DSP-02 · AC-DSP-02-05

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, thêm lịch sử CREATED, CATEGORY_CHANGED và ASSIGNED.

1. QL-A mở hồ sơ PB-A khi chưa có quyền chi tiết. NV-A và DP-All đọc hồ sơ đã giao lại có hai sự kiện cùng giờ. Mở màn hình tương ứng bằng NV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/staff/requests/{id}` với các biến thể đã nêu. So JSON history và các trường trên hồ sơ 315 trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** Quyền báo cáo riêng không cho đọc chi tiết. Người có quyền nhận đầy đủ trạng thái, version, bổ sung/kết quả và nội dung lịch sử đúng thứ tự giờ rồi ID. Lịch sử cũ giữ sau phân công.

**Kết quả:** Not Run.

## TC-DSP-02-06 — ID sai định dạng

**Yêu cầu kiểm:** FR-DSP-02 · AC-DSP-02-06

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, thêm lịch sử CREATED, CATEGORY_CHANGED và ASSIGNED.

1. Gọi GET chi tiết với ID 0 hoặc chữ. Mở màn hình tương ứng bằng NV-A, ghi tham số hoặc ID từ fixture.
2. Gọi `GET /api/staff/requests/{id}` với các biến thể đã nêu. So JSON history và các trường trên hồ sơ 315 trước/sau đổi bộ lọc hoặc actor theo tình huống.

**Mong đợi:** 422, không trả lịch sử hoặc thông tin sinh viên.

**Kết quả:** Not Run.

## TC-DSP-03-01 — Gán loại

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-01

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Yêu cầu Chưa xác định được gán Học vụ. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.

**Kết quả:** Not Run.

## TC-DSP-03-02 — Danh mục sai

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-02

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Gửi loại đã ngừng dùng hoặc ID không có. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; loại và lịch sử không thay đổi.

**Kết quả:** Not Run.

## TC-DSP-03-03 — Không có quyền

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Nhân viên thường hoặc sinh viên tự đổi loại. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; loại hiện hành giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-03-04 — Đã phân công / thao tác đồng thời

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-04

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-03-05 — Không đổi giá trị

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-05

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Gửi lại cùng giá trị hiện hành với version mới nhất. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 200 no_change; version và số history không tăng.

**Kết quả:** Not Run.

## TC-DSP-03-06 — Thiếu lý do đổi

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-06

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 422 reason; giá trị/version/history giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-03-07 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-07

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Đọc và lưu ảnh chụp requests.category_id/record_version và history CATEGORY_CHANGED trước thao tác. Dùng payload hợp lệ của actor DP-All.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/dispatch/requests/{id}/category` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests.category_id/record_version và history CATEGORY_CHANGED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-03-08 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-08

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. Đọc phiên bản trong requests.category_id/record_version và history CATEGORY_CHANGED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/dispatch/requests/{id}/category` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests.category_id/record_version và history CATEGORY_CHANGED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-03-09 — Giới hạn trạng thái và loại đích

**Yêu cầu kiểm:** FR-DSP-03 · AC-DSP-03-09

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, loại đích 101, reason “Chuyển đúng nghiệp vụ” nếu cần và version hiện hành.

1. DP-All phân loại hồ sơ Resolved, Closed và hồ sơ Received với loại đích Chưa xác định. Thử đổi loại hợp lệ với lý do 4000 và 4001 ký tự. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/category` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.category_id/record_version và history CATEGORY_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Hồ sơ kết thúc trả 409, loại đích không hợp lệ hoặc lý do quá dài trả 422. Đổi hợp lệ ghi CATEGORY_CHANGED và giữ trách nhiệm, trạng thái, hạn.

**Kết quả:** Not Run.

## TC-DSP-04-01 — Phân công lần đầu

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-01

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Giao yêu cầu cho NV-A thuộc PB-A. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.

**Kết quả:** Not Run.

## TC-DSP-04-02 — Người không thuộc đơn vị

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-02

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Chọn PB-A với nhân viên PB-B. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không tạo trách nhiệm không nhất quán.

**Kết quả:** Not Run.

## TC-DSP-04-03 — Tự chiếm yêu cầu

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Nhân viên không có quyền điều phối gửi assignee_id của mình. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không thay trách nhiệm.

**Kết quả:** Not Run.

## TC-DSP-04-04 — Đổi trách nhiệm đồng thời

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-04

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.

**Kết quả:** Not Run.

## TC-DSP-04-05 — Không đổi giá trị

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-05

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Gửi lại cùng giá trị hiện hành với version mới nhất. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 200 no_change; version và số history không tăng.

**Kết quả:** Not Run.

## TC-DSP-04-06 — Thiếu lý do đổi

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-06

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 422 reason; giá trị/version/history giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-04-07 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-07

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Đọc và lưu ảnh chụp requests.department_id/assignee_id/record_version và history ASSIGNED trước thao tác. Dùng payload hợp lệ của actor DP-All.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/dispatch/requests/{id}/assignment` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests.department_id/assignee_id/record_version và history ASSIGNED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-04-08 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-08

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Đọc phiên bản trong requests.department_id/assignee_id/record_version và history ASSIGNED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/dispatch/requests/{id}/assignment` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests.department_id/assignee_id/record_version và history ASSIGNED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-04-09 — Phân công lần đầu và giữ câu hỏi

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-09

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. DP-All giao hồ sơ Received chưa có người mà bỏ reason. Sau đó chuyển hồ sơ WaitingInfo 311 sang PB-B/NV-B có lý do. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lần đầu thành công. Khi chuyển, cùng ID, câu hỏi 401, hạn và lịch sử cũ. Quyền xử lý chuyển sang NV-B; nguồn RPT-05 chỉ có bộ trách nhiệm mới.

**Kết quả:** Not Run.

## TC-DSP-04-10 — Chặn phân công hồ sơ kết thúc

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-10

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Dùng hồ sơ 313 Resolved và 314 Closed, gửi bộ PB-B/NV-B hợp lệ, lý do đủ và phiên bản đúng. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 409 ở cả hai, không đổi phòng/người, status, result, version hay số sự kiện.

**Kết quả:** Not Run.

## TC-DSP-04-11 — Người hoặc đơn vị ngừng hoạt động

**Yêu cầu kiểm:** FR-DSP-04 · AC-DSP-04-11

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310 hoặc hồ sơ 311 khi đang chờ. Bộ chuẩn PB-A/NV-A = 201/111, bộ thay thế 202/112.

1. Thử department_id=299, assignee_id=199, ID 0 hoặc nhân viên không có quyền xử lý. Sau đó thử lý do đổi dài 4001 ký tự. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/assignment` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.department_id/assignee_id/record_version và history ASSIGNED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 422 từng lượt, không có bộ trách nhiệm nửa hợp lệ và không thêm lịch sử.

**Kết quả:** Not Run.

## TC-DSP-05-01 — Hạn hợp lệ

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-01

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Hạn hai ngày sau created_at. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.

**Kết quả:** Not Run.

## TC-DSP-05-02 — Hạn trước khi tạo

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-02

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. due_at nhỏ hơn created_at. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; hạn cũ giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-05-03 — Sai quyền

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Sinh viên hoặc nhân viên không điều phối sửa hạn. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không thay hạn.

**Kết quả:** Not Run.

## TC-DSP-05-04 — Không hạn / đúng ranh giới

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-04

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.

**Kết quả:** Not Run.

## TC-DSP-05-05 — Không đổi giá trị

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-05

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Gửi lại cùng giá trị hiện hành với version mới nhất. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 200 no_change; version và số history không tăng.

**Kết quả:** Not Run.

## TC-DSP-05-06 — Thiếu lý do đổi

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-06

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 422 reason; giá trị/version/history giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-05-07 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-07

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Đọc và lưu ảnh chụp requests.due_at/record_version và history DUE_CHANGED trước thao tác. Dùng payload hợp lệ của actor DP-All.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/dispatch/requests/{id}/due` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests.due_at/record_version và history DUE_CHANGED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-05-08 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-08

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Đọc phiên bản trong requests.due_at/record_version và history DUE_CHANGED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/dispatch/requests/{id}/due` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests.due_at/record_version và history DUE_CHANGED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-05-09 — Múi giờ và hạn bằng giờ tạo

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-09

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. Trên hồ sơ 310 có created_at 2026-12-01T01:00:00Z, đặt hạn 2026-12-01T08:00:00+07:00, sau đó thử chuỗi không có múi giờ. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lượt đầu lưu đúng 01:00Z và giữ owner/status; hạn thiếu múi giờ trả 422. Ghi thay đổi cùng version và lịch sử ở lượt hợp lệ.

**Kết quả:** Not Run.

## TC-DSP-05-10 — Chặn đặt hạn hồ sơ kết thúc

**Yêu cầu kiểm:** FR-DSP-05 · AC-DSP-05-10

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 310, hạn mẫu 2026-12-03T08:00:00+07:00 và version hiện hành.

1. DP-All thử đặt hạn cho 313 Resolved và 314 Closed với giờ hợp lệ và phiên bản hiện hành. Trên hồ sơ mở thử đổi hạn với lý do 4001 ký tự. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/due` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.due_at/record_version và history DUE_CHANGED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Hai hồ sơ kết thúc trả 409, lý do dài trả 422. Dữ liệu/hạn/version/lịch sử giữ như trước.

**Kết quả:** Not Run.

## TC-DSP-06-01 — Bắt đầu đúng

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-01

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 317 đã giao và version hiện hành.

1. NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/start` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/started_at/record_version và history STARTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.

**Kết quả:** Not Run.

## TC-DSP-06-02 — Trạng thái sai

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-02

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 317 đã giao và version hiện hành.

1. Bắt đầu hồ sơ Đã đóng. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/start` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/started_at/record_version và history STARTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không thay trạng thái và thời điểm.

**Kết quả:** Not Run.

## TC-DSP-06-03 — Người khác thao tác

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-B. Dùng 317 đã giao và version hiện hành.

1. NV-B bắt đầu hồ sơ giao NV-A. Dùng NV-B hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/start` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/started_at/record_version và history STARTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; trách nhiệm giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-06-04 — Bấm hai lần

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-04

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 317 đã giao và version hiện hành.

1. Gửi lặp lần bắt đầu. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/start` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/started_at/record_version và history STARTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-06-05 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-05

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 317 đã giao và version hiện hành.

1. Đọc và lưu ảnh chụp requests.status/started_at/record_version và history STARTED trước thao tác. Dùng payload hợp lệ của actor NV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/staff/requests/{id}/start` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests.status/started_at/record_version và history STARTED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-06-06 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-06

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 317 đã giao và version hiện hành.

1. Đọc phiên bản trong requests.status/started_at/record_version và history STARTED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/staff/requests/{id}/start` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests.status/started_at/record_version và history STARTED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-06-07 — Hồ sơ chưa có trách nhiệm

**Yêu cầu kiểm:** FR-DSP-06 · AC-DSP-06-07

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 317 đã giao và version hiện hành.

1. NV-A thử bắt đầu hồ sơ 310 chưa phân công rồi kiểm lại hồ sơ hợp lệ đã bắt đầu. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/start` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/started_at/record_version và history STARTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lượt không có quyền đối tượng không thay dữ liệu. Với hồ sơ hợp lệ, started_at lấy đồng hồ máy chủ lần đầu, giữ due_at và student_id.

**Kết quả:** Not Run.

## TC-DSP-07-01 — Câu hỏi hợp lệ

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-01

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. Đang xử lý, hỏi Vui lòng cung cấp mã lớp. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/questions` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions và history INFO_REQUESTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.

**Kết quả:** Not Run.

## TC-DSP-07-02 — Câu hỏi rỗng

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-02

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. Nội dung chỉ có khoảng trắng. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/questions` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions và history INFO_REQUESTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Không lưu câu hỏi hoặc chuyển trạng thái.

**Kết quả:** Not Run.

## TC-DSP-07-03 — Ngoài trách nhiệm

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-B. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. NV-B yêu cầu bổ sung hồ sơ giao NV-A. Dùng NV-B hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/questions` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions và history INFO_REQUESTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không lộ thêm dữ liệu.

**Kết quả:** Not Run.

## TC-DSP-07-04 — Đã có câu hỏi mở

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-04

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. Gửi lần thứ hai khi Chờ bổ sung. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/questions` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions và history INFO_REQUESTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** HTTP 409 INVALID_STATE; vẫn một câu hỏi mở, không có history/question thứ hai.

**Kết quả:** Not Run.

## TC-DSP-07-05 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-05

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. Đọc và lưu ảnh chụp requests, questions và history INFO_REQUESTED trước thao tác. Dùng payload hợp lệ của actor NV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/staff/requests/{id}/questions` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests, questions và history INFO_REQUESTED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-07-06 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-06

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. Đọc phiên bản trong requests, questions và history INFO_REQUESTED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/staff/requests/{id}/questions` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests, questions và history INFO_REQUESTED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-07-07 — Đợt bổ sung tiếp theo

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-07

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. Sau STU-04 đóng câu hỏi 1 và Processing, NV hỏi câu hỏi 2. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/questions` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions và history INFO_REQUESTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Có câu hỏi 2 ID khác, một câu hỏi mở; câu hỏi 1/answer cũ giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-07-08 — Không hỏi khi mới tiếp nhận

**Yêu cầu kiểm:** FR-DSP-07 · AC-DSP-07-08

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, question “Vui lòng cung cấp mã lớp” và version hiện hành.

1. NV-A thử tạo câu hỏi trên hồ sơ Received đã được giao, sau đó chuyển sang Processing và thử câu hỏi 4001 ký tự. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/questions` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, questions và history INFO_REQUESTED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Received trả 409, văn bản quá dài trả 422. Không đổi trạng thái/hạn/thời điểm bắt đầu, không sinh question/history.

**Kết quả:** Not Run.

## TC-DSP-08-01 — Tiến độ công khai

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-01

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. Ghi Đang kiểm tra thủ tục, visibility=public. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/progress` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, progress_notes và history với phạm vi công khai/nội bộ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.

**Kết quả:** Not Run.

## TC-DSP-08-02 — Giá trị sai

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-02

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. Nội dung trống hoặc visibility=secret. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/progress` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, progress_notes và history với phạm vi công khai/nội bộ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không thêm lịch sử.

**Kết quả:** Not Run.

## TC-DSP-08-03 — Mất trách nhiệm

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-B. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. Dùng NV-B hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/progress` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, progress_notes và history với phạm vi công khai/nội bộ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không ghi nội dung mới.

**Kết quả:** Not Run.

## TC-DSP-08-04 — Tiến độ nội bộ

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-04

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. Ghi ghi chú internal có dấu TEST-INTERNAL. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/progress` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, progress_notes và history với phạm vi công khai/nội bộ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.

**Kết quả:** Not Run.

## TC-DSP-08-05 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-05

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. Đọc và lưu ảnh chụp requests, progress_notes và history với phạm vi công khai/nội bộ trước thao tác. Dùng payload hợp lệ của actor NV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/staff/requests/{id}/progress` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests, progress_notes và history với phạm vi công khai/nội bộ với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-08-06 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-06

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. Đọc phiên bản trong requests, progress_notes và history với phạm vi công khai/nội bộ, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/staff/requests/{id}/progress` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests, progress_notes và history với phạm vi công khai/nội bộ đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-08-07 — Mặc định nội bộ và bảo toàn luồng

**Yêu cầu kiểm:** FR-DSP-08 · AC-DSP-08-07

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315, content “Đã kiểm tra hồ sơ”, visibility=public và version hiện hành.

1. Trên hồ sơ WaitingInfo 311, NV-A gửi hai ghi chú cùng nội dung bằng hai thao tác với phiên bản mới, lần đầu bỏ visibility. Thử thêm nội dung 4001 ký tự và hồ sơ Closed. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/progress` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, progress_notes và history với phạm vi công khai/nội bộ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Ghi chú mặc định internal, hai thao tác riêng có hai mục. Giữ nguyên câu hỏi, hạn, trạng thái và người xử lý. Trả 422 cho nội dung quá dài và 409 cho Closed.

**Kết quả:** Not Run.

## TC-DSP-09-01 — Có kết quả

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-01

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/resolve` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results và history RESOLVED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.

**Kết quả:** Not Run.

## TC-DSP-09-02 — Còn thiếu thông tin / kết quả trống

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-02

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. Chờ bổ sung hoặc resolution rỗng. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/resolve` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results và history RESOLVED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.

**Kết quả:** Not Run.

## TC-DSP-09-03 — Sai người xử lý

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-B. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. NV-B ghi kết quả cho hồ sơ NV-A. Dùng NV-B hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/resolve` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results và history RESOLVED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không thay kết quả.

**Kết quả:** Not Run.

## TC-DSP-09-04 — Gửi lại / dữ liệu cũ

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-04

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. Gửi lại cùng thao tác hoặc dùng record_version cũ. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/resolve` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results và history RESOLVED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.

**Kết quả:** Not Run.

## TC-DSP-09-05 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-05

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. Đọc và lưu ảnh chụp requests, results và history RESOLVED trước thao tác. Dùng payload hợp lệ của actor NV-A.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/staff/requests/{id}/resolve` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests, results và history RESOLVED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-09-06 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-06

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. Đọc phiên bản trong requests, results và history RESOLVED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/staff/requests/{id}/resolve` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests, results và history RESOLVED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-09-07 — Không giải quyết khi chờ

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-07

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. WaitingInfo có question mở; NV gửi result_content. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/resolve` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results và history RESOLVED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 409 INVALID_STATE; không result và không resolved_at mới.

**Kết quả:** Not Run.

## TC-DSP-09-08 — Bảo toàn hồ sơ khi có kết quả

**Yêu cầu kiểm:** FR-DSP-09 · AC-DSP-09-08

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 315 không có câu hỏi mở, result_content “Đã xác nhận thông tin” và version hiện hành.

1. NV-A giải quyết hồ sơ 315 đang Processing với nội dung 4000 ký tự rồi thử lặp. Trên fixture độc lập thử nội dung 4001 ký tự. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/staff/requests/{id}/resolve` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests, results và history RESOLVED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Một kết quả và một RESOLVED công khai. Giữ hạn, giờ bắt đầu, trách nhiệm, không tự đóng hoặc tạo phản hồi. Lặp trả 409, quá dài trả 422.

**Kết quả:** Not Run.

## TC-DSP-10-01 — Đóng đúng

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-01

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 313 có kết quả 501 và version hiện hành.

1. Điều phối viên đóng yêu cầu Đã giải quyết có kết quả. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/close` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/closed_at/record_version, results và history CLOSED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.

**Kết quả:** Not Run.

## TC-DSP-10-02 — Đóng trước khi giải quyết

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-02

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 313 có kết quả 501 và version hiện hành.

1. Hồ sơ Đang xử lý chưa có kết quả. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/close` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/closed_at/record_version, results và history CLOSED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; không bỏ qua bước giải quyết.

**Kết quả:** Not Run.

## TC-DSP-10-03 — Sai vai trò

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-03

**Chuẩn bị:** Fixture độc lập. Actor chính NV-A. Dùng 313 có kết quả 501 và version hiện hành.

1. Sinh viên hoặc nhân viên thường gọi thao tác đóng. Dùng NV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/close` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/closed_at/record_version, results và history CLOSED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Từ chối; trạng thái không đổi.

**Kết quả:** Not Run.

## TC-DSP-10-04 — Đóng lặp

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-04

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 313 có kết quả 501 và version hiện hành.

1. Gửi lại thao tác đóng; sau đó thử ghi tiến độ. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/close` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/closed_at/record_version, results và history CLOSED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.

**Kết quả:** Not Run.

## TC-DSP-10-05 — Rollback giao dịch

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-05

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 313 có kết quả 501 và version hiện hành.

1. Đọc và lưu ảnh chụp requests.status/closed_at/record_version, results và history CLOSED trước thao tác. Dùng payload hợp lệ của actor DP-All.
2. Bật điểm lỗi ghi history trên build thử rồi gọi `POST /api/dispatch/requests/{id}/close` một lần. Xác nhận log đã qua ghi nghiệp vụ trước lỗi.
3. Tắt giả lập. Đọc từ kết nối khác, so requests.status/closed_at/record_version, results và history CLOSED với trước thao tác. Lưu HTTP 500, mã đối chiếu và kết quả so sánh.

**Mong đợi:** 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

**Kết quả:** Not Run.

## TC-DSP-10-06 — Cập nhật phiên bản cũ

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-06

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 313 có kết quả 501 và version hiện hành.

1. Đọc phiên bản trong requests.status/closed_at/record_version, results và history CLOSED, tạo hai payload cùng version.
2. Gửi payload A hợp lệ qua `POST /api/dispatch/requests/{id}/close` và giữ phản hồi thành công cùng dữ liệu sau commit. Gửi payload B có thay đổi khác với version ban đầu.
3. Kiểm 409 STALE_VERSION, giao diện yêu cầu tải lại và requests.status/closed_at/record_version, results và history CLOSED đúng như sau A.

**Mong đợi:** Tab B nhận 409 STALE_VERSION; không ghi đè dữ liệu/lịch sử của A; giao diện yêu cầu tải lại.

**Kết quả:** Not Run.

## TC-DSP-10-07 — Điều kiện đóng và giữ dữ liệu

**Yêu cầu kiểm:** FR-DSP-10 · AC-DSP-10-07

**Chuẩn bị:** Fixture độc lập. Actor chính DP-All. Dùng 313 có kết quả 501 và version hiện hành.

1. DP-All đóng hồ sơ 313 Resolved có result nhưng chưa có feedback. Trên fixture âm độc lập thử Resolved thiếu result. Dùng DP-All hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/dispatch/requests/{id}/close` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm requests.status/closed_at/record_version, results và history CLOSED theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Hồ sơ hợp lệ đóng và giữ nguyên result/dữ liệu. Fixture thiếu kết quả bị chặn. Không tạo feedback tự động, không tăng version khi bị từ chối.

**Kết quả:** Not Run.

## TC-RPT-01-01 — Đối chiếu phép tính

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-01

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Tạo 10 yêu cầu: 2 mỗi trạng thái. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/01` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Mỗi nhóm bằng 2, tổng bằng 10.

**Kết quả:** Not Run.

## TC-RPT-01-02 — Bộ lọc không hợp lệ

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-02

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. from_date sau to_date hoặc đơn vị không tồn tại. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/01` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

**Kết quả:** Not Run.

## TC-RPT-01-03 — Vượt quyền báo cáo

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-03

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đăng nhập QL-A, gọi `GET /api/reports/01` lọc department_id=202. Đổi sang actor NV-A và thử lại không có lọc.
2. Đối chiếu 403 và JSON không chứa số đếm, mã nguồn hoặc thông tin nội bộ.

**Mong đợi:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

**Kết quả:** Not Run.

## TC-RPT-01-04 — Không có yêu cầu

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-04

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Không có yêu cầu. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/01` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng.

**Kết quả:** Not Run.

## TC-RPT-01-05 — Ranh giới kỳ

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-05

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/01` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

**Kết quả:** Not Run.

## TC-RPT-01-06 — Join không nhân bản

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-06

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chụp tập source_references, sample_count, metrics và bộ lọc thực áp dụng của lần gọi `GET /api/reports/01` với fixture gốc.
2. Thêm hai history và hai ghi chú cho một hồ sơ thuộc mẫu. Gọi lại cùng actor/bộ lọc.
3. Đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng trước/sau và kiểm mã nguồn chỉ tính một lần.

**Mong đợi:** Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

**Kết quả:** Not Run.

## TC-RPT-01-07 — Trạng thái hiện tại và nhóm bằng không

**Yêu cầu kiểm:** FR-RPT-01 · AC-RPT-01-07

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-01 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Tạo hồ sơ trong kỳ, chuyển Received sang Processing sau cuối kỳ rồi đo kỳ cũ. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/01` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Hồ sơ được nhóm theo Processing hiện tại, có đủ năm nhóm, nhóm không có nguồn đếm 0, tổng đúng mẫu.

**Kết quả:** Not Run.

## TC-RPT-02-01 — Đối chiếu phép tính

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-01

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Có một yêu cầu mở quá hạn, một đúng hạn, một không hạn, một Đã giải quyết quá hạn. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/02` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Chỉ yêu cầu mở quá hạn xuất hiện.

**Kết quả:** Not Run.

## TC-RPT-02-02 — Tham số không hỗ trợ

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-02

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Gửi from_date/to_date vào báo cáo backlog hiện hành. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/02` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.

**Kết quả:** Not Run.

## TC-RPT-02-03 — Vượt quyền báo cáo

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-03

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đăng nhập QL-A, gọi `GET /api/reports/02` lọc department_id=202. Đổi sang actor NV-A và thử lại không có lọc.
2. Đối chiếu 403 và JSON không chứa số đếm, mã nguồn hoặc thông tin nội bộ.

**Mong đợi:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

**Kết quả:** Not Run.

## TC-RPT-02-04 — now bằng due_at

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-04

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. now bằng due_at. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/02` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Yêu cầu chưa bị tính quá hạn tại đúng ranh giới.

**Kết quả:** Not Run.

## TC-RPT-02-05 — Hồ sơ cũ còn mở

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-05

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Một request từ tháng trước còn mở trong scope; due_at nhỏ hơn as_of. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/02` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.

**Kết quả:** Not Run.

## TC-RPT-02-06 — Join không nhân bản

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-06

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chụp tập source_references, sample_count, metrics và bộ lọc thực áp dụng của lần gọi `GET /api/reports/02` với fixture gốc.
2. Thêm hai history và hai ghi chú cho một hồ sơ thuộc mẫu. Gọi lại cùng actor/bộ lọc.
3. Đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng trước/sau và kiểm mã nguồn chỉ tính một lần.

**Mong đợi:** Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

**Kết quả:** Not Run.

## TC-RPT-02-07 — Nguồn rỗng và thứ tự quá hạn

**Yêu cầu kiểm:** FR-RPT-02 · AC-RPT-02-07

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-02 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đo tập trống rồi đo ba hồ sơ quá hạn, hai hồ sơ cùng due_at. Thêm một hồ sơ Closed, một hồ sơ không hạn và một hồ sơ đúng as_of. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/02` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Tập trống có sample_count và overdue_count bằng 0. Ba dòng quá hạn sắp theo hạn rồi ID tăng, giờ quá hạn tính theo cùng một as_of. Không đưa ba hồ sơ đối chứng vào nguồn.

**Kết quả:** Not Run.

## TC-RPT-03-01 — Đối chiếu phép tính

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-01

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Hai hồ sơ có thời gian 2 giờ và 4 giờ; một hồ sơ còn mở. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/03` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại.

**Kết quả:** Not Run.

## TC-RPT-03-02 — Bộ lọc không hợp lệ

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-02

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. from_date sau to_date hoặc đơn vị không tồn tại. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/03` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

**Kết quả:** Not Run.

## TC-RPT-03-03 — Vượt quyền báo cáo

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-03

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đăng nhập QL-A, gọi `GET /api/reports/03` lọc department_id=202. Đổi sang actor NV-A và thử lại không có lọc.
2. Đối chiếu 403 và JSON không chứa số đếm, mã nguồn hoặc thông tin nội bộ.

**Mong đợi:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

**Kết quả:** Not Run.

## TC-RPT-03-04 — Không hồ sơ đã giải quyết

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-04

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Không hồ sơ đã giải quyết. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/03` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ.

**Kết quả:** Not Run.

## TC-RPT-03-05 — Ranh giới kỳ

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-05

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Hai record có resolved_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/03` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

**Kết quả:** Not Run.

## TC-RPT-03-06 — Join không nhân bản

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-06

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chụp tập source_references, sample_count, metrics và bộ lọc thực áp dụng của lần gọi `GET /api/reports/03` với fixture gốc.
2. Thêm hai history và hai ghi chú cho một hồ sơ thuộc mẫu. Gọi lại cùng actor/bộ lọc.
3. Đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng trước/sau và kiểm mã nguồn chỉ tính một lần.

**Mong đợi:** Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

**Kết quả:** Not Run.

## TC-RPT-03-07 — Độ chính xác giờ xử lý

**Yêu cầu kiểm:** FR-RPT-03 · AC-RPT-03-07

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-03 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Tạo hai hồ sơ có khoảng tạo đến giải quyết 3618 và 3636 giây, một hồ sơ đi qua WaitingInfo. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/03` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Tính trung bình từ giá trị chưa làm tròn, giữ thời gian chờ trong mẫu. UI hiện hai chữ số thập phân, nhãn giờ từ gửi đến giải quyết.

**Kết quả:** Not Run.

## TC-RPT-04-01 — Đối chiếu phép tính

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-01

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Ba Học vụ, hai CNTT và một Chưa xác định. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/04` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần.

**Kết quả:** Not Run.

## TC-RPT-04-02 — Bộ lọc không hợp lệ

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-02

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. from_date sau to_date hoặc đơn vị không tồn tại. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/04` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

**Kết quả:** Not Run.

## TC-RPT-04-03 — Vượt quyền báo cáo

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-03

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đăng nhập QL-A, gọi `GET /api/reports/04` lọc department_id=202. Đổi sang actor NV-A và thử lại không có lọc.
2. Đối chiếu 403 và JSON không chứa số đếm, mã nguồn hoặc thông tin nội bộ.

**Mong đợi:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

**Kết quả:** Not Run.

## TC-RPT-04-04 — Hai nhóm bằng số lượng

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-04

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Hai nhóm bằng số lượng. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/04` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Nhóm count bằng nhau sắp category_id tăng dần; không bỏ Chưa xác định.

**Kết quả:** Not Run.

## TC-RPT-04-05 — Ranh giới kỳ

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-05

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/04` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

**Kết quả:** Not Run.

## TC-RPT-04-06 — Join không nhân bản

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-06

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chụp tập source_references, sample_count, metrics và bộ lọc thực áp dụng của lần gọi `GET /api/reports/04` với fixture gốc.
2. Thêm hai history và hai ghi chú cho một hồ sơ thuộc mẫu. Gọi lại cùng actor/bộ lọc.
3. Đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng trước/sau và kiểm mã nguồn chỉ tính một lần.

**Mong đợi:** Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

**Kết quả:** Not Run.

## TC-RPT-04-07 — Loại hiện hành và nhóm mặc định

**Yêu cầu kiểm:** FR-RPT-04 · AC-RPT-04-07

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-04 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Tạo ba hồ sơ trong kỳ; một hồ sơ đổi loại sau cuối kỳ và hồ sơ khác giữ Chưa xác định. Đo lại kỳ cũ. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/04` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Nhóm theo danh mục hiện hành, vẫn tính Chưa xác định. Tổng nhóm bằng sample_count.

**Kết quả:** Not Run.

## TC-RPT-05-01 — Đối chiếu phép tính

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-01

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. PB-A có 2 hồ sơ mở, 1 hồ sơ Resolved; PB-B có 1 mở; 1 hồ sơ chưa giao. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/05` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Backlog PB-A=2, PB-B=1, chưa giao=1, tổng 4; request Resolved không tính.

**Kết quả:** Not Run.

## TC-RPT-05-02 — Tham số không hỗ trợ

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-02

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Gửi from_date/to_date vào báo cáo backlog hiện hành. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/05` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.

**Kết quả:** Not Run.

## TC-RPT-05-03 — Vượt quyền báo cáo

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-03

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đăng nhập QL-A, gọi `GET /api/reports/05` lọc department_id=202. Đổi sang actor NV-A và thử lại không có lọc.
2. Đối chiếu 403 và JSON không chứa số đếm, mã nguồn hoặc thông tin nội bộ.

**Mong đợi:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

**Kết quả:** Not Run.

## TC-RPT-05-04 — Chuyển một hồ sơ PB-A sang PB-B

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-04

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chuyển một hồ sơ PB-A sang PB-B. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/05` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Sau chuyển, tổng toàn hệ thống giữ nguyên; chỉ nhóm hiện hành tăng/giảm tương ứng.

**Kết quả:** Not Run.

## TC-RPT-05-05 — Hồ sơ cũ còn mở

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-05

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Một request từ tháng trước còn mở trong scope; được giao PB-A. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/05` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.

**Kết quả:** Not Run.

## TC-RPT-05-06 — Join không nhân bản

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-06

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chụp tập source_references, sample_count, metrics và bộ lọc thực áp dụng của lần gọi `GET /api/reports/05` với fixture gốc.
2. Thêm hai history và hai ghi chú cho một hồ sơ thuộc mẫu. Gọi lại cùng actor/bộ lọc.
3. Đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng trước/sau và kiểm mã nguồn chỉ tính một lần.

**Mong đợi:** Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

**Kết quả:** Not Run.

## TC-RPT-05-07 — Nhóm chưa giao và tập rỗng

**Yêu cầu kiểm:** FR-RPT-05 · AC-RPT-05-07

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-05 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. QL-All và QL-A xem tập có hồ sơ chưa giao, hồ sơ mở PB-A và hồ sơ đã đóng. Lọc danh mục không có hồ sơ. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/05` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Chỉ QL-All thấy nhóm chưa giao có ID phòng/người rỗng. Không tính hồ sơ đã đóng. Tập trống open_total=0.

**Kết quả:** Not Run.

## TC-RPT-06-01 — Đối chiếu phép tính

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-01

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Có điểm 5 và 3; yêu cầu chưa đánh giá không tính. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/06` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.

**Kết quả:** Not Run.

## TC-RPT-06-02 — Bộ lọc không hợp lệ

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-02

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. from_date sau to_date hoặc đơn vị không tồn tại. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/06` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

**Kết quả:** Not Run.

## TC-RPT-06-03 — Vượt quyền báo cáo

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-03

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Đăng nhập QL-A, gọi `GET /api/reports/06` lọc department_id=202. Đổi sang actor NV-A và thử lại không có lọc.
2. Đối chiếu 403 và JSON không chứa số đếm, mã nguồn hoặc thông tin nội bộ.

**Mong đợi:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

**Kết quả:** Not Run.

## TC-RPT-06-04 — Chưa có phản hồi hoặc đổi điểm

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-04

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chưa có phản hồi hoặc đổi điểm. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/06` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.

**Kết quả:** Not Run.

## TC-RPT-06-05 — Ranh giới kỳ

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-05

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Hai record có feedback.updated_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/06` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

**Kết quả:** Not Run.

## TC-RPT-06-06 — Join không nhân bản

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-06

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Chụp tập source_references, sample_count, metrics và bộ lọc thực áp dụng của lần gọi `GET /api/reports/06` với fixture gốc.
2. Thêm hai history và hai ghi chú cho một hồ sơ thuộc mẫu. Gọi lại cùng actor/bộ lọc.
3. Đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng trước/sau và kiểm mã nguồn chỉ tính một lần.

**Mong đợi:** Chỉ tính feedback hiện hành của request đó một lần, không nhân bản bởi dữ liệu con.

**Kết quả:** Not Run.

## TC-RPT-06-07 — Sửa điểm và kỳ cập nhật

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-07

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Feedback4 tại kỳA đổi thành3 tại kỳB không trùng A. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/06` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Cùng feedback_id/request_id; kỳA không còn dòng này, kỳB có điểm3; mẫu toàn thời gian vẫn1.

**Kết quả:** Not Run.

## TC-RPT-06-08 — Phân bố đủ năm mức

**Yêu cầu kiểm:** FR-RPT-06 · AC-RPT-06-08

**Chuẩn bị:** Fixture độc lập. Actor chính QL-A. Tạo tập nguồn của RPT-06 theo [fixture báo cáo](fixtures.md#nguồn-tính-báo-cáo).

1. Tạo phản hồi 1 và 5, một hồ sơ không có phản hồi, rồi lọc bộ dữ liệu không có mẫu. Dùng giá trị nguồn, đồng hồ hoặc biến thể ghi trong fixture.
2. Gọi `GET /api/reports/06` bằng QL-A; đối chiếu tập source_references, sample_count, metrics và bộ lọc thực áp dụng với dữ liệu test theo công thức riêng của FR. Khi từ chối, đối chiếu status và trường sai.

**Mong đợi:** Mẫu là 2, mean_score=3.00, có đủ năm nhóm điểm, tổng nhóm là 2. Không tính hồ sơ chưa đánh giá. Không mẫu thì mean_score=null.

**Kết quả:** Not Run.

## TC-IAM-01-01 — Đúng tài khoản

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.

**Kết quả:** Not Run.

## TC-IAM-01-02 — Sai thông tin

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Sai mật khẩu hoặc email không có. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Thông báo chung; không tạo phiên được phép nghiệp vụ.

**Kết quả:** Not Run.

## TC-IAM-01-03 — Chưa xác thực

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.

**Kết quả:** Not Run.

## TC-IAM-01-04 — Cố định phiên / hết hạn

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Dùng mã phiên trước đăng nhập hoặc phiên hết hạn idle 30 phút, absolute 8 giờ. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.

**Kết quả:** Not Run.

## TC-IAM-01-05 — Tài khoản inactive

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Đúng password nhưng active=false. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** 401 INVALID_CREDENTIALS như email không có; không cấp session.

**Kết quả:** Not Run.

## TC-IAM-01-06 — Giới hạn thử sai

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. 5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn.

**Kết quả:** Not Run.

## TC-IAM-01-07 — Biên dữ liệu đăng nhập

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Dùng email seed với khoảng trắng đầu/cuối và chữ hoa. Gửi email sai định dạng hoặc 255 ký tự, mật khẩu rỗng hoặc 257 ký tự, rồi nối khoảng trắng vào mật khẩu seed hợp lệ. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Email đúng được chuẩn hóa. Dữ liệu sai báo 422. Mật khẩu có phần thêm bị đối chiếu nguyên văn và trả 401 nếu sai. Không ghi mật khẩu vào log.

**Kết quả:** Not Run.

## TC-IAM-01-08 — Thuộc tính cookie và phiên

**Yêu cầu kiểm:** FR-IAM-01 · AC-IAM-01-08

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Dùng email sv-a@example.test và mật khẩu test cấp từ seed. Với case giới hạn, cô lập bộ đếm email/IP.

1. Trên cấu hình HTTPS thử đăng nhập đúng một actor seed, đọc thuộc tính cookie và gọi một điểm truy cập có bảo vệ sau mốc idle và sau mốc tuyệt đối. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/login` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm mã HTTP, cookie, log và API được bảo vệ theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Cookie HttpOnly, SameSite=Lax, Secure khi HTTPS. Phiên cũ đổi khi đăng nhập, các phiên quá hạn idle hoặc tuyệt đối bị từ chối.

**Kết quả:** Not Run.

## TC-IAM-02-01 — Đăng xuất đúng

**Yêu cầu kiểm:** FR-IAM-02 · AC-IAM-02-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo hai phiên độc lập của SV-A, giữ cookie A/B để đối chiếu khi cần.

1. Đăng nhập rồi chọn Đăng xuất. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/logout` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm status, cookie, hiệu lực phiên và dữ liệu cache theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Trở về đăng nhập; phiên máy chủ bị vô hiệu.

**Kết quả:** Not Run.

## TC-IAM-02-02 — Không có phiên

**Yêu cầu kiểm:** FR-IAM-02 · AC-IAM-02-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo hai phiên độc lập của SV-A, giữ cookie A/B để đối chiếu khi cần.

1. Gọi đăng xuất khi đã thoát. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/logout` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm status, cookie, hiệu lực phiên và dữ liệu cache theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.

**Kết quả:** Not Run.

## TC-IAM-02-03 — Dùng lại phiên

**Yêu cầu kiểm:** FR-IAM-02 · AC-IAM-02-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo hai phiên độc lập của SV-A, giữ cookie A/B để đối chiếu khi cần.

1. Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/logout` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm status, cookie, hiệu lực phiên và dữ liệu cache theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Bị chặn; dữ liệu không thay đổi.

**Kết quả:** Not Run.

## TC-IAM-02-04 — Back / đăng nhập lại

**Yêu cầu kiểm:** FR-IAM-02 · AC-IAM-02-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo hai phiên độc lập của SV-A, giữ cookie A/B để đối chiếu khi cần.

1. Back trang trước rồi đăng nhập lại. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/logout` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm status, cookie, hiệu lực phiên và dữ liệu cache theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.

**Kết quả:** Not Run.

## TC-IAM-02-05 — Đăng xuất hai phiên và CSRF

**Yêu cầu kiểm:** FR-IAM-02 · AC-IAM-02-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo hai phiên độc lập của SV-A, giữ cookie A/B để đối chiếu khi cần.

1. Mở hai phiên độc lập của SV-A. Đăng xuất phiên A với token thiếu/sai rồi với token đúng. Thử đọc hồ sơ bằng cả hai phiên. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/logout` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm status, cookie, hiệu lực phiên và dữ liệu cache theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Token sai trả 403, A còn hiệu lực. Token đúng trả 204, A bị vô hiệu và xóa cookie, B vẫn có quyền. Kiểm header private, no-store ở màn hình dữ liệu.

**Kết quả:** Not Run.

## TC-IAM-02-06 — Xử lý lỗi máy chủ khi đăng xuất

**Yêu cầu kiểm:** FR-IAM-02 · AC-IAM-02-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Tạo hai phiên độc lập của SV-A, giữ cookie A/B để đối chiếu khi cần.

1. Giả lập lỗi lưu trạng thái phiên trước khi xác nhận logout ở môi trường test, rồi thử lại bình thường. Dùng SV-A hoặc actor đối chứng được nêu trong tình huống.
2. Gửi qua `POST /api/auth/logout` với các trường thay đổi đã nêu. Với nhiều lượt, ghi riêng payload, status và ID/phiên bản trả về.
3. Kiểm status, cookie, hiệu lực phiên và dữ liệu cache theo expected; nếu bị từ chối, đọc lại để xác nhận dữ liệu nghiệp vụ giữ nguyên.

**Mong đợi:** Lượt lỗi trả 500 có mã đối chiếu, không báo đã thoát. Lượt sau trả 204 và phiên bị vô hiệu, không lộ thông tin trong lỗi.

**Kết quả:** Not Run.

## TC-IAM-03-01 — Quyền phù hợp

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-01

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.

**Kết quả:** Not Run.

## TC-IAM-03-02 — Vai trò giả

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-02

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: Gửi role=manager hoặc scope=all trong dữ liệu.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.

**Kết quả:** Not Run.

## TC-IAM-03-03 — Đối tượng của người khác

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-03

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.

**Kết quả:** Not Run.

## TC-IAM-03-04 — Thay trách nhiệm / dữ liệu nội bộ

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-04

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.

**Kết quả:** Not Run.

## TC-IAM-03-05 — CSRF thiếu/sai

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-05

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: Đăng nhập hợp lệ rồi gửi POST protected không CSRF hoặc CSRF của phiên khác.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** 403 CSRF_INVALID; không ghi dữ liệu. GET chỉ đọc không yêu cầu CSRF.

**Kết quả:** Not Run.

## TC-IAM-03-06 — Aggregate vượt scope

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-06

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: QL-A chỉ PB-A; gọi report PB-B hoặc scope=all.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** PB-B trả 403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A.

**Kết quả:** Not Run.

## TC-IAM-03-07 — Phiên hết hạn hoặc tài khoản không hoạt động

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-07

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: Tạo phiên hợp lệ, sau đó đổi tài khoản seed sang không hoạt động ở DB hoặc đưa đồng hồ quá giới hạn phiên. Dùng cookie cũ gọi GET và POST hợp lệ.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** 401 và không đọc/ghi dữ liệu. Middleware lấy trạng thái hiện hành từ máy chủ.

**Kết quả:** Not Run.

## TC-IAM-03-08 — Quyền điều phối và ID không tồn tại

**Yêu cầu kiểm:** FR-IAM-03 · AC-IAM-03-08

**Chuẩn bị:** Fixture độc lập. Actor chính SV-A. Nạp các phiên SV-A, SV-B, NV-A, NV-B, DP-All, DP-A, QL-A và QL-All theo tình huống.

1. Dùng phiên và quyền của tình huống: Dùng DP-A và DP-All đọc hàng chờ có hồ sơ chưa giao. Dùng SV-B gọi chi tiết hồ sơ SV-A và một ID hợp lệ không tồn tại.
2. Chọn API của hành vi: GET /api/student/requests/310 để đọc theo chủ, POST /api/staff/requests/315/progress để ghi, GET /api/dispatch/requests cho điều phối hoặc GET /api/reports/01 cho phạm vi báo cáo.
3. Gọi bằng từng phiên tách riêng. Đối chiếu mã HTTP, phạm vi JSON và trạng thái DB trước/sau; lưu response đã loại thông tin phiên.

**Mong đợi:** Chỉ DP-All thấy chưa giao. Hai phản hồi 404 cho SV-B có nội dung tương đương, không tiết lộ hồ sơ có tồn tại.

**Kết quả:** Not Run.
