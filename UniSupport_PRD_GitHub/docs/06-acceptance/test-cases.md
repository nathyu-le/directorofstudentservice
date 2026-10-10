# Test case chức năng

Fixture giả lập vàendpoint ở [data](../02-domain/data-dictionary.md)/[API](../02-domain/api-contract.md). Casebulk ở JSON cósteps để nhập Master. Chưa thực thi:mọi result Not Run, actual/evidence để trống.

## TC-STU-01-01 — Đủ trường, loại hợp lệ

FR:FR-STU-01; AC:AC-STU-01-01.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.

Expected:Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-02 — Thiếu nội dung

FR:FR-STU-01; AC:AC-STU-01-02.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Tiêu đề chỉ có khoảng trắng hoặc mô tả trống.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Tiêu đề chỉ có khoảng trắng hoặc mô tả trống.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo tại trường; không tạo yêu cầu hay lịch sử một phần.

Expected:Báo tại trường; không tạo yêu cầu hay lịch sử một phần.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-03 — Giả mạo chủ yêu cầu

FR:FR-STU-01; AC:AC-STU-01-03.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Phiên SV-A gửi thêm student_id=SV-B.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Phiên SV-A gửi thêm student_id=SV-B.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.

Expected:Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-04 — Gửi lặp

FR:FR-STU-01; AC:AC-STU-01-04.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Gửi lại cùng submission_key hai lần.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Gửi lại cùng submission_key hai lần.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Trả cùng mã; tổng số yêu cầu tăng một, không hai.

Expected:Trả cùng mã; tổng số yêu cầu tăng một, không hai.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-05 — Mất response rồi retry

FR:FR-STU-01; AC:AC-STU-01-05.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Commit tạo xong nhưng client không nhận response; gửi lại cùng khóa/payload.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Commit tạo xong nhưng client không nhận response; gửi lại cùng khóa/payload.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: HTTP 200 cùng reference; DB 1 request và 1 event CREATED cho khóa.

Expected:HTTP 200 cùng reference; DB 1 request và 1 event CREATED cho khóa.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-06 — Khóa bị tái sử dụng sai

FR:FR-STU-01; AC:AC-STU-01-06.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Cùng student/key nhưng đổi description.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Cùng student/key nhưng đổi description.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 409 SUBMISSION_KEY_CONFLICT; bản đã tạo không đổi.

Expected:409 SUBMISSION_KEY_CONFLICT; bản đã tạo không đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-07 — Hai ý định riêng

FR:FR-STU-01; AC:AC-STU-01-07.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Cùng nội dung nhưng hai UUID khác nhau.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Cùng nội dung nhưng hai UUID khác nhau.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Tạo2 yêu cầu hợp lệ; không chống trùng theo nội dung để làm mất ý định mới.

Expected:Tạo2 yêu cầu hợp lệ; không chống trùng theo nội dung để làm mất ý định mới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-01-08 — Rollback giao dịch

FR:FR-STU-01; AC:AC-STU-01-08.

Preconditions:Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-01.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-02-01 — Có yêu cầu và bộ lọc

FR:FR-STU-02; AC:AC-STU-02-01.

Preconditions:Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

Dữ liệu:SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-02.
2. Thiết lập chính xác dữ liệu: SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý.
3. Thực hiện GET /api/student/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.

Expected:Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-02-02 — Tham số sai

FR:FR-STU-02; AC:AC-STU-02-02.

Preconditions:Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

Dữ liệu:page=0 hoặc trạng thái không thuộc danh sách.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-02.
2. Thiết lập chính xác dữ liệu: page=0 hoặc trạng thái không thuộc danh sách.
3. Thực hiện GET /api/student/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.

Expected:Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-02-03 — Lọc theo người khác

FR:FR-STU-02; AC:AC-STU-02-03.

Preconditions:Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

Dữ liệu:SV-A sửa tham số student_id thành SV-B.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-02.
2. Thiết lập chính xác dữ liệu: SV-A sửa tham số student_id thành SV-B.
3. Thực hiện GET /api/student/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không trả bất kỳ yêu cầu của SV-B.

Expected:Không trả bất kỳ yêu cầu của SV-B.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-02-04 — Danh sách rỗng

FR:FR-STU-02; AC:AC-STU-02-04.

Preconditions:Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

Dữ liệu:Tài khoản SV-C chưa có yêu cầu.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-02.
2. Thiết lập chính xác dữ liệu: Tài khoản SV-C chưa có yêu cầu.
3. Thực hiện GET /api/student/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.

Expected:Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-02-05 — Trang vượt cuối

FR:FR-STU-02; AC:AC-STU-02-05.

Preconditions:Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu.

Dữ liệu:Có 1 request, page=2 với page_size 20.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-02.
2. Thiết lập chính xác dữ liệu: Có 1 request, page=2 với page_size 20.
3. Thực hiện GET /api/student/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: items=[] và total=1; không trả request người khác, không tự sửa page.

Expected:items=[] và total=1; không trả request người khác, không tự sửa page.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-03-01 — Tiến độ đã cập nhật

FR:FR-STU-03; AC:AC-STU-03-01.

Preconditions:Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

Dữ liệu:Yêu cầu được phân công rồi chuyển Đang xử lý.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-03.
2. Thiết lập chính xác dữ liệu: Yêu cầu được phân công rồi chuyển Đang xử lý.
3. Thực hiện GET /api/student/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.

Expected:Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-03-02 — Mã không tồn tại

FR:FR-STU-03; AC:AC-STU-03-02.

Preconditions:Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

Dữ liệu:Mã yêu cầu không có trong bộ dữ liệu.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-03.
2. Thiết lập chính xác dữ liệu: Mã yêu cầu không có trong bộ dữ liệu.
3. Thực hiện GET /api/student/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.

Expected:Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-03-03 — Truy cập ngang

FR:FR-STU-03; AC:AC-STU-03-03.

Preconditions:Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

Dữ liệu:SV-B mở URL yêu cầu của SV-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-03.
2. Thiết lập chính xác dữ liệu: SV-B mở URL yêu cầu của SV-A.
3. Thực hiện GET /api/student/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A.

Expected:Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-03-04 — Chưa phân công / chờ bổ sung

FR:FR-STU-03; AC:AC-STU-03-04.

Preconditions:Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

Dữ liệu:Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-03.
2. Thiết lập chính xác dữ liệu: Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung.
3. Thực hiện GET /api/student/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.

Expected:Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-03-05 — Không lộ internal JSON

FR:FR-STU-03; AC:AC-STU-03-05.

Preconditions:Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập.

Dữ liệu:Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-03.
2. Thiết lập chính xác dữ liệu: Ghi internal TEST-INTERNAL rồi SV gọi endpoint trực tiếp.
3. Thực hiện GET /api/student/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history.

Expected:JSON không chứa nội dung TEST-INTERNAL hoặc các trường internal_history.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-01 — Trả lời hợp lệ

FR:FR-STU-04; AC:AC-STU-04-01.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.

Expected:Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-02 — Nội dung trống

FR:FR-STU-04; AC:AC-STU-04-02.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:Trả lời chỉ có khoảng trắng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: Trả lời chỉ có khoảng trắng.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.

Expected:Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-03 — Sai chủ

FR:FR-STU-04; AC:AC-STU-04-03.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:SV-B gửi câu trả lời cho yêu cầu SV-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: SV-B gửi câu trả lời cho yêu cầu SV-A.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; nội dung và trạng thái không đổi.

Expected:Từ chối; nội dung và trạng thái không đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-04 — Gửi từ màn hình cũ

FR:FR-STU-04; AC:AC-STU-04-04.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.

Expected:Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-05 — Tiếp tục không reset mốc

FR:FR-STU-04; AC:AC-STU-04-05.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:Ghi lại started_at, due_at, owner trước khi trả lời question mở.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: Ghi lại started_at, due_at, owner trước khi trả lời question mở.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Sau bổ sung cả 3 giá trị giữ nguyên, Processing,1answer cho question.

Expected:Sau bổ sung cả 3 giá trị giữ nguyên, Processing,1answer cho question.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-06 — Rollback giao dịch

FR:FR-STU-04; AC:AC-STU-04-06.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-04-07 — Cập nhật phiên bản cũ

FR:FR-STU-04; AC:AC-STU-04-07.

Preconditions:Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-04.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/student/requests/{id}/answers theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-01 — Phản hồi hợp lệ

FR:FR-STU-05; AC:AC-STU-05-01.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.

Expected:Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-02 — Điểm sai / chưa có kết quả

FR:FR-STU-05; AC:AC-STU-05-02.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không có phản hồi mới để tính báo cáo.

Expected:Từ chối; không có phản hồi mới để tính báo cáo.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-03 — Không phải chủ

FR:FR-STU-05; AC:AC-STU-05-03.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:SV-B gửi đánh giá cho yêu cầu SV-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: SV-B gửi đánh giá cho yêu cầu SV-A.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; phản hồi của SV-A không bị sửa.

Expected:Từ chối; phản hồi của SV-A không bị sửa.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-04 — Cập nhật phản hồi

FR:FR-STU-05; AC:AC-STU-05-04.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:SV-A đổi điểm từ 4 thành 3.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: SV-A đổi điểm từ 4 thành 3.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.

Expected:Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-05 — Phản hồi khi đã đóng

FR:FR-STU-05; AC:AC-STU-05-05.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:Chủ SV gửi score 5 trên Closed có result.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: Chủ SV gửi score 5 trên Closed có result.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Lưu feedback, status vẫn Closed, closed_at không đổi.

Expected:Lưu feedback, status vẫn Closed, closed_at không đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-06 — Rollback giao dịch

FR:FR-STU-05; AC:AC-STU-05-06.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-STU-05-07 — Cập nhật phiên bản cũ

FR:FR-STU-05; AC:AC-STU-05-07.

Preconditions:Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-STU-05.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/student/requests/{id}/feedback theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-01-01 — Yêu cầu mới

FR:FR-DSP-01; AC:AC-DSP-01-01.

Preconditions:Có quyền điều phối trong phạm vi được cấu hình.

Dữ liệu:Tạo yêu cầu Chưa xác định bằng STU-01.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-01.
2. Thiết lập chính xác dữ liệu: Tạo yêu cầu Chưa xác định bằng STU-01.
3. Thực hiện GET /api/dispatch/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.

Expected:Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-01-02 — Lọc sai

FR:FR-DSP-01; AC:AC-DSP-01-02.

Preconditions:Có quyền điều phối trong phạm vi được cấu hình.

Dữ liệu:category_id không tồn tại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-01.
2. Thiết lập chính xác dữ liệu: category_id không tồn tại.
3. Thực hiện GET /api/dispatch/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo tham số sai; không áp dụng loại khác âm thầm.

Expected:Báo tham số sai; không áp dụng loại khác âm thầm.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-01-03 — Vai trò không được phép

FR:FR-DSP-01; AC:AC-DSP-01-03.

Preconditions:Có quyền điều phối trong phạm vi được cấu hình.

Dữ liệu:Sinh viên gọi trang/điểm truy cập hàng chờ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-01.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi trang/điểm truy cập hàng chờ.
3. Thực hiện GET /api/dispatch/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không lộ danh sách toàn trường.

Expected:Từ chối; không lộ danh sách toàn trường.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-01-04 — Sau phân công

FR:FR-DSP-01; AC:AC-DSP-01-04.

Preconditions:Có quyền điều phối trong phạm vi được cấu hình.

Dữ liệu:Phân công yêu cầu rồi lọc unassigned=true.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-01.
2. Thiết lập chính xác dữ liệu: Phân công yêu cầu rồi lọc unassigned=true.
3. Thực hiện GET /api/dispatch/requests theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.

Expected:Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-02-01 — Có lịch sử

FR:FR-DSP-02; AC:AC-DSP-02-01.

Preconditions:Có quyền trên yêu cầu theo vai trò và phạm vi công việc.

Dữ liệu:Yêu cầu đã tiếp nhận, phân loại, phân công.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-02.
2. Thiết lập chính xác dữ liệu: Yêu cầu đã tiếp nhận, phân loại, phân công.
3. Thực hiện GET /api/staff/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.

Expected:Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-02-02 — Mã không có

FR:FR-DSP-02; AC:AC-DSP-02-02.

Preconditions:Có quyền trên yêu cầu theo vai trò và phạm vi công việc.

Dữ liệu:Mở mã không tồn tại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-02.
2. Thiết lập chính xác dữ liệu: Mở mã không tồn tại.
3. Thực hiện GET /api/staff/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không tìm thấy; không sinh dữ liệu.

Expected:Không tìm thấy; không sinh dữ liệu.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-02-03 — Ngoài phạm vi

FR:FR-DSP-02; AC:AC-DSP-02-03.

Preconditions:Có quyền trên yêu cầu theo vai trò và phạm vi công việc.

Dữ liệu:Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-02.
2. Thiết lập chính xác dữ liệu: Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A.
3. Thực hiện GET /api/staff/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối, không trả nội dung sinh viên.

Expected:Từ chối, không trả nội dung sinh viên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-02-04 — Ghi chú nội bộ

FR:FR-DSP-02; AC:AC-DSP-02-04.

Preconditions:Có quyền trên yêu cầu theo vai trò và phạm vi công việc.

Dữ liệu:Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-02.
2. Thiết lập chính xác dữ liệu: Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03.
3. Thực hiện GET /api/staff/requests/{id} theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.

Expected:Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-01 — Gán loại

FR:FR-DSP-03; AC:AC-DSP-03-01.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Yêu cầu Chưa xác định được gán Học vụ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Yêu cầu Chưa xác định được gán Học vụ.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.

Expected:Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-02 — Danh mục sai

FR:FR-DSP-03; AC:AC-DSP-03-02.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Gửi loại đã ngừng dùng hoặc ID không có.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Gửi loại đã ngừng dùng hoặc ID không có.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; loại và lịch sử không thay đổi.

Expected:Từ chối; loại và lịch sử không thay đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-03 — Không có quyền

FR:FR-DSP-03; AC:AC-DSP-03-03.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Nhân viên thường hoặc sinh viên tự đổi loại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Nhân viên thường hoặc sinh viên tự đổi loại.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; loại hiện hành giữ nguyên.

Expected:Từ chối; loại hiện hành giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-04 — Đã phân công / thao tác đồng thời

FR:FR-DSP-03; AC:AC-DSP-03-04.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.

Expected:Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-05 — Không đổi giá trị

FR:FR-DSP-03; AC:AC-DSP-03-05.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Gửi lại cùng giá trị hiện hành với version mới nhất.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Gửi lại cùng giá trị hiện hành với version mới nhất.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 200 no_change; version và số history không tăng.

Expected:200 no_change; version và số history không tăng.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-06 — Thiếu lý do đổi

FR:FR-DSP-03; AC:AC-DSP-03-06.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 422 reason; giá trị/version/history giữ nguyên.

Expected:422 reason; giá trị/version/history giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-07 — Rollback giao dịch

FR:FR-DSP-03; AC:AC-DSP-03-07.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-03-08 — Cập nhật phiên bản cũ

FR:FR-DSP-03; AC:AC-DSP-03-08.

Preconditions:Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-03.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/dispatch/requests/{id}/category theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-01 — Phân công lần đầu

FR:FR-DSP-04; AC:AC-DSP-04-01.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Giao yêu cầu cho NV-A thuộc PB-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Giao yêu cầu cho NV-A thuộc PB-A.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.

Expected:Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-02 — Người không thuộc đơn vị

FR:FR-DSP-04; AC:AC-DSP-04-02.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Chọn PB-A với nhân viên PB-B.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Chọn PB-A với nhân viên PB-B.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không tạo trách nhiệm không nhất quán.

Expected:Từ chối; không tạo trách nhiệm không nhất quán.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-03 — Tự chiếm yêu cầu

FR:FR-DSP-04; AC:AC-DSP-04-03.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Nhân viên không có quyền điều phối gửi assignee_id của mình.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Nhân viên không có quyền điều phối gửi assignee_id của mình.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không thay trách nhiệm.

Expected:Từ chối; không thay trách nhiệm.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-04 — Đổi trách nhiệm đồng thời

FR:FR-DSP-04; AC:AC-DSP-04-04.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.

Expected:Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-05 — Không đổi giá trị

FR:FR-DSP-04; AC:AC-DSP-04-05.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Gửi lại cùng giá trị hiện hành với version mới nhất.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Gửi lại cùng giá trị hiện hành với version mới nhất.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 200 no_change; version và số history không tăng.

Expected:200 no_change; version và số history không tăng.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-06 — Thiếu lý do đổi

FR:FR-DSP-04; AC:AC-DSP-04-06.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 422 reason; giá trị/version/history giữ nguyên.

Expected:422 reason; giá trị/version/history giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-07 — Rollback giao dịch

FR:FR-DSP-04; AC:AC-DSP-04-07.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-04-08 — Cập nhật phiên bản cũ

FR:FR-DSP-04; AC:AC-DSP-04-08.

Preconditions:Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-04.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/dispatch/requests/{id}/assignment theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-01 — Hạn hợp lệ

FR:FR-DSP-05; AC:AC-DSP-05-01.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Hạn hai ngày sau created_at.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Hạn hai ngày sau created_at.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.

Expected:Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-02 — Hạn trước khi tạo

FR:FR-DSP-05; AC:AC-DSP-05-02.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:due_at nhỏ hơn created_at.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: due_at nhỏ hơn created_at.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; hạn cũ giữ nguyên.

Expected:Từ chối; hạn cũ giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-03 — Sai quyền

FR:FR-DSP-05; AC:AC-DSP-05-03.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Sinh viên hoặc nhân viên không điều phối sửa hạn.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Sinh viên hoặc nhân viên không điều phối sửa hạn.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không thay hạn.

Expected:Từ chối; không thay hạn.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-04 — Không hạn / đúng ranh giới

FR:FR-DSP-05; AC:AC-DSP-05-04.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.

Expected:Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-05 — Không đổi giá trị

FR:FR-DSP-05; AC:AC-DSP-05-05.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Gửi lại cùng giá trị hiện hành với version mới nhất.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Gửi lại cùng giá trị hiện hành với version mới nhất.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 200 no_change; version và số history không tăng.

Expected:200 no_change; version và số history không tăng.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-06 — Thiếu lý do đổi

FR:FR-DSP-05; AC:AC-DSP-05-06.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Đã có giá trị xác định; đổi sang giá trị khác, reason chỉ khoảng trắng.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 422 reason; giá trị/version/history giữ nguyên.

Expected:422 reason; giá trị/version/history giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-07 — Rollback giao dịch

FR:FR-DSP-05; AC:AC-DSP-05-07.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-05-08 — Cập nhật phiên bản cũ

FR:FR-DSP-05; AC:AC-DSP-05-08.

Preconditions:Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-05.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/dispatch/requests/{id}/due theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-06-01 — Bắt đầu đúng

FR:FR-DSP-06; AC:AC-DSP-06-01.

Preconditions:Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

Dữ liệu:NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-06.
2. Thiết lập chính xác dữ liệu: NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình.
3. Thực hiện POST /api/staff/requests/{id}/start theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.

Expected:Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-06-02 — Trạng thái sai

FR:FR-DSP-06; AC:AC-DSP-06-02.

Preconditions:Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

Dữ liệu:Bắt đầu hồ sơ Đã đóng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-06.
2. Thiết lập chính xác dữ liệu: Bắt đầu hồ sơ Đã đóng.
3. Thực hiện POST /api/staff/requests/{id}/start theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không thay trạng thái và thời điểm.

Expected:Từ chối; không thay trạng thái và thời điểm.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-06-03 — Người khác thao tác

FR:FR-DSP-06; AC:AC-DSP-06-03.

Preconditions:Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

Dữ liệu:NV-B bắt đầu hồ sơ giao NV-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-06.
2. Thiết lập chính xác dữ liệu: NV-B bắt đầu hồ sơ giao NV-A.
3. Thực hiện POST /api/staff/requests/{id}/start theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; trách nhiệm giữ nguyên.

Expected:Từ chối; trách nhiệm giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-06-04 — Bấm hai lần

FR:FR-DSP-06; AC:AC-DSP-06-04.

Preconditions:Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

Dữ liệu:Gửi lặp lần bắt đầu.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-06.
2. Thiết lập chính xác dữ liệu: Gửi lặp lần bắt đầu.
3. Thực hiện POST /api/staff/requests/{id}/start theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.

Expected:Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-06-05 — Rollback giao dịch

FR:FR-DSP-06; AC:AC-DSP-06-05.

Preconditions:Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-06.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/staff/requests/{id}/start theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-06-06 — Cập nhật phiên bản cũ

FR:FR-DSP-06; AC:AC-DSP-06-06.

Preconditions:Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-06.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/staff/requests/{id}/start theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-01 — Câu hỏi hợp lệ

FR:FR-DSP-07; AC:AC-DSP-07-01.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:Đang xử lý, hỏi Vui lòng cung cấp mã lớp.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: Đang xử lý, hỏi Vui lòng cung cấp mã lớp.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.

Expected:Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-02 — Câu hỏi rỗng

FR:FR-DSP-07; AC:AC-DSP-07-02.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:Nội dung chỉ có khoảng trắng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: Nội dung chỉ có khoảng trắng.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không lưu câu hỏi hoặc chuyển trạng thái.

Expected:Không lưu câu hỏi hoặc chuyển trạng thái.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-03 — Ngoài trách nhiệm

FR:FR-DSP-07; AC:AC-DSP-07-03.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:NV-B yêu cầu bổ sung hồ sơ giao NV-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: NV-B yêu cầu bổ sung hồ sơ giao NV-A.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không lộ thêm dữ liệu.

Expected:Từ chối; không lộ thêm dữ liệu.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-04 — Đã có câu hỏi mở

FR:FR-DSP-07; AC:AC-DSP-07-04.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:Gửi lần thứ hai khi Chờ bổ sung.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: Gửi lần thứ hai khi Chờ bổ sung.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: HTTP 409 INVALID_STATE; vẫn1question mở, không có history/question thứ hai.

Expected:HTTP 409 INVALID_STATE; vẫn1question mở, không có history/question thứ hai.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-05 — Rollback giao dịch

FR:FR-DSP-07; AC:AC-DSP-07-05.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-06 — Cập nhật phiên bản cũ

FR:FR-DSP-07; AC:AC-DSP-07-06.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-07-07 — Đợt bổ sung tiếp theo

FR:FR-DSP-07; AC:AC-DSP-07-07.

Preconditions:Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

Dữ liệu:Sau STU-04 đóng question1 và Processing, NV hỏi question2.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-07.
2. Thiết lập chính xác dữ liệu: Sau STU-04 đóng question1 và Processing, NV hỏi question2.
3. Thực hiện POST /api/staff/requests/{id}/questions theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Có question2 ID khác,1 question mở; question1/answer cũ giữ nguyên.

Expected:Có question2 ID khác,1 question mở; question1/answer cũ giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-08-01 — Tiến độ công khai

FR:FR-DSP-08; AC:AC-DSP-08-01.

Preconditions:Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

Dữ liệu:Ghi Đang kiểm tra thủ tục, visibility=public.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-08.
2. Thiết lập chính xác dữ liệu: Ghi Đang kiểm tra thủ tục, visibility=public.
3. Thực hiện POST /api/staff/requests/{id}/progress theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.

Expected:Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-08-02 — Giá trị sai

FR:FR-DSP-08; AC:AC-DSP-08-02.

Preconditions:Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

Dữ liệu:Nội dung trống hoặc visibility=secret.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-08.
2. Thiết lập chính xác dữ liệu: Nội dung trống hoặc visibility=secret.
3. Thực hiện POST /api/staff/requests/{id}/progress theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không thêm lịch sử.

Expected:Từ chối; không thêm lịch sử.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-08-03 — Mất trách nhiệm

FR:FR-DSP-08; AC:AC-DSP-08-03.

Preconditions:Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

Dữ liệu:NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-08.
2. Thiết lập chính xác dữ liệu: NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B.
3. Thực hiện POST /api/staff/requests/{id}/progress theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không ghi nội dung mới.

Expected:Từ chối; không ghi nội dung mới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-08-04 — Tiến độ nội bộ

FR:FR-DSP-08; AC:AC-DSP-08-04.

Preconditions:Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

Dữ liệu:Ghi ghi chú internal có dấu TEST-INTERNAL.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-08.
2. Thiết lập chính xác dữ liệu: Ghi ghi chú internal có dấu TEST-INTERNAL.
3. Thực hiện POST /api/staff/requests/{id}/progress theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.

Expected:Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-08-05 — Rollback giao dịch

FR:FR-DSP-08; AC:AC-DSP-08-05.

Preconditions:Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-08.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/staff/requests/{id}/progress theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-08-06 — Cập nhật phiên bản cũ

FR:FR-DSP-08; AC:AC-DSP-08-06.

Preconditions:Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-08.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/staff/requests/{id}/progress theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-01 — Có kết quả

FR:FR-DSP-09; AC:AC-DSP-09-01.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.

Expected:Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-02 — Còn thiếu thông tin / kết quả trống

FR:FR-DSP-09; AC:AC-DSP-09-02.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:Chờ bổ sung hoặc resolution rỗng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: Chờ bổ sung hoặc resolution rỗng.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.

Expected:Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-03 — Sai người xử lý

FR:FR-DSP-09; AC:AC-DSP-09-03.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:NV-B ghi kết quả cho hồ sơ NV-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: NV-B ghi kết quả cho hồ sơ NV-A.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không thay kết quả.

Expected:Từ chối; không thay kết quả.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-04 — Gửi lại / dữ liệu cũ

FR:FR-DSP-09; AC:AC-DSP-09-04.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:Gửi lại cùng thao tác hoặc dùng record_version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: Gửi lại cùng thao tác hoặc dùng record_version cũ.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.

Expected:Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-05 — Rollback giao dịch

FR:FR-DSP-09; AC:AC-DSP-09-05.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-06 — Cập nhật phiên bản cũ

FR:FR-DSP-09; AC:AC-DSP-09-06.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-09-07 — Không giải quyết khi chờ

FR:FR-DSP-09; AC:AC-DSP-09-07.

Preconditions:Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

Dữ liệu:WaitingInfo có question mở; NV gửi result_content.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-09.
2. Thiết lập chính xác dữ liệu: WaitingInfo có question mở; NV gửi result_content.
3. Thực hiện POST /api/staff/requests/{id}/resolve theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 409 INVALID_STATE; không result và không resolved_at mới.

Expected:409 INVALID_STATE; không result và không resolved_at mới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-10-01 — Đóng đúng

FR:FR-DSP-10; AC:AC-DSP-10-01.

Preconditions:Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

Dữ liệu:Điều phối viên đóng yêu cầu Đã giải quyết có kết quả.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-10.
2. Thiết lập chính xác dữ liệu: Điều phối viên đóng yêu cầu Đã giải quyết có kết quả.
3. Thực hiện POST /api/dispatch/requests/{id}/close theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.

Expected:Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-10-02 — Đóng trước khi giải quyết

FR:FR-DSP-10; AC:AC-DSP-10-02.

Preconditions:Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

Dữ liệu:Hồ sơ Đang xử lý chưa có kết quả.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-10.
2. Thiết lập chính xác dữ liệu: Hồ sơ Đang xử lý chưa có kết quả.
3. Thực hiện POST /api/dispatch/requests/{id}/close theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không bỏ qua bước giải quyết.

Expected:Từ chối; không bỏ qua bước giải quyết.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-10-03 — Sai vai trò

FR:FR-DSP-10; AC:AC-DSP-10-03.

Preconditions:Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

Dữ liệu:Sinh viên hoặc nhân viên thường gọi thao tác đóng.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-10.
2. Thiết lập chính xác dữ liệu: Sinh viên hoặc nhân viên thường gọi thao tác đóng.
3. Thực hiện POST /api/dispatch/requests/{id}/close theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; trạng thái không đổi.

Expected:Từ chối; trạng thái không đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-10-04 — Đóng lặp

FR:FR-DSP-10; AC:AC-DSP-10-04.

Preconditions:Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

Dữ liệu:Gửi lại thao tác đóng; sau đó thử ghi tiến độ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-10.
2. Thiết lập chính xác dữ liệu: Gửi lại thao tác đóng; sau đó thử ghi tiến độ.
3. Thực hiện POST /api/dispatch/requests/{id}/close theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.

Expected:Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-10-05 — Rollback giao dịch

FR:FR-DSP-10; AC:AC-DSP-10-05.

Preconditions:Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

Dữ liệu:Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-10.
2. Thiết lập chính xác dữ liệu: Trên môi trường test, ép lỗi ghi history sau khi thao tác dữ liệu chính, không áp dụng vào production.
3. Thực hiện POST /api/dispatch/requests/{id}/close theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Expected:500 INTERNAL_ERROR không chi tiết DB; dữ liệu/version/history rollback về trước thao tác, không có bản ghi mồ côi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-DSP-10-06 — Cập nhật phiên bản cũ

FR:FR-DSP-10; AC:AC-DSP-10-06.

Preconditions:Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

Dữ liệu:Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-DSP-10.
2. Thiết lập chính xác dữ liệu: Hai tab đọc cùng version; tabA lưu hợp lệ, tabB gửi version cũ.
3. Thực hiện POST /api/dispatch/requests/{id}/close theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Expected:TabB409 STALE_VERSION; không ghi đè dữ liệu/lịch sử củaA; giao diện yêu cầu tải lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-01-01 — Đối chiếu phép tính

FR:FR-RPT-01; AC:AC-RPT-01-01.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Tạo 10 yêu cầu: 2 mỗi trạng thái.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-01.
2. Thiết lập chính xác dữ liệu: Tạo 10 yêu cầu: 2 mỗi trạng thái.
3. Thực hiện GET /api/reports/01 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Mỗi nhóm bằng 2, tổng bằng 10.

Expected:Mỗi nhóm bằng 2, tổng bằng 10.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-01-02 — Bộ lọc không hợp lệ

FR:FR-RPT-01; AC:AC-RPT-01-02.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:from_date sau to_date hoặc đơn vị không tồn tại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-01.
2. Thiết lập chính xác dữ liệu: from_date sau to_date hoặc đơn vị không tồn tại.
3. Thực hiện GET /api/reports/01 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Expected:Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-01-03 — Vượt quyền báo cáo

FR:FR-RPT-01; AC:AC-RPT-01-03.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-01.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
3. Thực hiện GET /api/reports/01 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Expected:Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-01-04 — Không có yêu cầu

FR:FR-RPT-01; AC:AC-RPT-01-04.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Không có yêu cầu

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-01.
2. Thiết lập chính xác dữ liệu: Không có yêu cầu
3. Thực hiện GET /api/reports/01 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng.

Expected:Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-01-05 — Ranh giới kỳ

FR:FR-RPT-01; AC:AC-RPT-01-05.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-01.
2. Thiết lập chính xác dữ liệu: Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.
3. Thực hiện GET /api/reports/01 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Expected:Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-01-06 — Join không nhân bản

FR:FR-RPT-01; AC:AC-RPT-01-06.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request có 3 notes,2 questions và 5 history events.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-01.
2. Thiết lập chính xác dữ liệu: Một request có 3 notes,2 questions và 5 history events.
3. Thực hiện GET /api/reports/01 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Expected:Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-02-01 — Đối chiếu phép tính

FR:FR-RPT-02; AC:AC-RPT-02-01.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Có một yêu cầu mở quá hạn, một đúng hạn, một không hạn, một Đã giải quyết quá hạn.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-02.
2. Thiết lập chính xác dữ liệu: Có một yêu cầu mở quá hạn, một đúng hạn, một không hạn, một Đã giải quyết quá hạn.
3. Thực hiện GET /api/reports/02 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ yêu cầu mở quá hạn xuất hiện.

Expected:Chỉ yêu cầu mở quá hạn xuất hiện.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-02-02 — Tham số không hỗ trợ

FR:FR-RPT-02; AC:AC-RPT-02-02.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Gửi from_date/to_date vào báo cáo backlog hiện hành.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-02.
2. Thiết lập chính xác dữ liệu: Gửi from_date/to_date vào báo cáo backlog hiện hành.
3. Thực hiện GET /api/reports/02 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.

Expected:HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-02-03 — Vượt quyền báo cáo

FR:FR-RPT-02; AC:AC-RPT-02-03.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-02.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
3. Thực hiện GET /api/reports/02 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Expected:Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-02-04 — now bằng due_at

FR:FR-RPT-02; AC:AC-RPT-02-04.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:now bằng due_at

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-02.
2. Thiết lập chính xác dữ liệu: now bằng due_at
3. Thực hiện GET /api/reports/02 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Yêu cầu chưa bị tính quá hạn tại đúng ranh giới.

Expected:Yêu cầu chưa bị tính quá hạn tại đúng ranh giới.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-02-05 — Hồ sơ cũ còn mở

FR:FR-RPT-02; AC:AC-RPT-02-05.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request từ tháng trước còn mở trong scope; due_at nhỏ hơn as_of.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-02.
2. Thiết lập chính xác dữ liệu: Một request từ tháng trước còn mở trong scope; due_at nhỏ hơn as_of.
3. Thực hiện GET /api/reports/02 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.

Expected:Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-02-06 — Join không nhân bản

FR:FR-RPT-02; AC:AC-RPT-02-06.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request có 3 notes,2 questions và 5 history events.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-02.
2. Thiết lập chính xác dữ liệu: Một request có 3 notes,2 questions và 5 history events.
3. Thực hiện GET /api/reports/02 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Expected:Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-03-01 — Đối chiếu phép tính

FR:FR-RPT-03; AC:AC-RPT-03-01.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Hai hồ sơ có thời gian 2 giờ và 4 giờ; một hồ sơ còn mở.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-03.
2. Thiết lập chính xác dữ liệu: Hai hồ sơ có thời gian 2 giờ và 4 giờ; một hồ sơ còn mở.
3. Thực hiện GET /api/reports/03 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại.

Expected:Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-03-02 — Bộ lọc không hợp lệ

FR:FR-RPT-03; AC:AC-RPT-03-02.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:from_date sau to_date hoặc đơn vị không tồn tại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-03.
2. Thiết lập chính xác dữ liệu: from_date sau to_date hoặc đơn vị không tồn tại.
3. Thực hiện GET /api/reports/03 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Expected:Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-03-03 — Vượt quyền báo cáo

FR:FR-RPT-03; AC:AC-RPT-03-03.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-03.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
3. Thực hiện GET /api/reports/03 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Expected:Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-03-04 — Không hồ sơ đã giải quyết

FR:FR-RPT-03; AC:AC-RPT-03-04.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Không hồ sơ đã giải quyết

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-03.
2. Thiết lập chính xác dữ liệu: Không hồ sơ đã giải quyết
3. Thực hiện GET /api/reports/03 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ.

Expected:Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-03-05 — Ranh giới kỳ

FR:FR-RPT-03; AC:AC-RPT-03-05.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Hai record có resolved_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-03.
2. Thiết lập chính xác dữ liệu: Hai record có resolved_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.
3. Thực hiện GET /api/reports/03 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Expected:Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-03-06 — Join không nhân bản

FR:FR-RPT-03; AC:AC-RPT-03-06.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request có 3 notes,2 questions và 5 history events.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-03.
2. Thiết lập chính xác dữ liệu: Một request có 3 notes,2 questions và 5 history events.
3. Thực hiện GET /api/reports/03 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Expected:Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-04-01 — Đối chiếu phép tính

FR:FR-RPT-04; AC:AC-RPT-04-01.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Ba Học vụ, hai CNTT và một Chưa xác định.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-04.
2. Thiết lập chính xác dữ liệu: Ba Học vụ, hai CNTT và một Chưa xác định.
3. Thực hiện GET /api/reports/04 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần.

Expected:Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-04-02 — Bộ lọc không hợp lệ

FR:FR-RPT-04; AC:AC-RPT-04-02.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:from_date sau to_date hoặc đơn vị không tồn tại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-04.
2. Thiết lập chính xác dữ liệu: from_date sau to_date hoặc đơn vị không tồn tại.
3. Thực hiện GET /api/reports/04 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Expected:Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-04-03 — Vượt quyền báo cáo

FR:FR-RPT-04; AC:AC-RPT-04-03.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-04.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
3. Thực hiện GET /api/reports/04 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Expected:Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-04-04 — Hai nhóm bằng số lượng

FR:FR-RPT-04; AC:AC-RPT-04-04.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Hai nhóm bằng số lượng

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-04.
2. Thiết lập chính xác dữ liệu: Hai nhóm bằng số lượng
3. Thực hiện GET /api/reports/04 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Nhóm count bằng nhau sắp category_id tăng dần; không bỏ Chưa xác định.

Expected:Nhóm count bằng nhau sắp category_id tăng dần; không bỏ Chưa xác định.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-04-05 — Ranh giới kỳ

FR:FR-RPT-04; AC:AC-RPT-04-05.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-04.
2. Thiết lập chính xác dữ liệu: Hai record có created_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.
3. Thực hiện GET /api/reports/04 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Expected:Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-04-06 — Join không nhân bản

FR:FR-RPT-04; AC:AC-RPT-04-06.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request có 3 notes,2 questions và 5 history events.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-04.
2. Thiết lập chính xác dữ liệu: Một request có 3 notes,2 questions và 5 history events.
3. Thực hiện GET /api/reports/04 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Expected:Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-05-01 — Đối chiếu phép tính

FR:FR-RPT-05; AC:AC-RPT-05-01.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:PB-A có 2 mở,1Resolved; PB-B có 1 mở;1 chưa giao.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-05.
2. Thiết lập chính xác dữ liệu: PB-A có 2 mở,1Resolved; PB-B có 1 mở;1 chưa giao.
3. Thực hiện GET /api/reports/05 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Backlog PB-A=2, PB-B=1, chưa giao=1, tổng4; request Resolved không tính.

Expected:Backlog PB-A=2, PB-B=1, chưa giao=1, tổng4; request Resolved không tính.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-05-02 — Tham số không hỗ trợ

FR:FR-RPT-05; AC:AC-RPT-05-02.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Gửi from_date/to_date vào báo cáo backlog hiện hành.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-05.
2. Thiết lập chính xác dữ liệu: Gửi from_date/to_date vào báo cáo backlog hiện hành.
3. Thực hiện GET /api/reports/05 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.

Expected:HTTP 422 UNSUPPORTED_FILTER; không lọc sai để giấu hồ sơ cũ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-05-03 — Vượt quyền báo cáo

FR:FR-RPT-05; AC:AC-RPT-05-03.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-05.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
3. Thực hiện GET /api/reports/05 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Expected:Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-05-04 — Chuyển một hồ sơ PB-A sang PB-B

FR:FR-RPT-05; AC:AC-RPT-05-04.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Chuyển một hồ sơ PB-A sang PB-B

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-05.
2. Thiết lập chính xác dữ liệu: Chuyển một hồ sơ PB-A sang PB-B
3. Thực hiện GET /api/reports/05 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Sau chuyển, tổng toàn hệ thống giữ nguyên; chỉ nhóm hiện hành tăng/giảm tương ứng.

Expected:Sau chuyển, tổng toàn hệ thống giữ nguyên; chỉ nhóm hiện hành tăng/giảm tương ứng.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-05-05 — Hồ sơ cũ còn mở

FR:FR-RPT-05; AC:AC-RPT-05-05.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request từ tháng trước còn mở trong scope; được giao PB-A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-05.
2. Thiết lập chính xác dữ liệu: Một request từ tháng trước còn mở trong scope; được giao PB-A.
3. Thực hiện GET /api/reports/05 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.

Expected:Vẫn được tính trong backlog hiện hành; không loại vì ngày tạo cũ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-05-06 — Join không nhân bản

FR:FR-RPT-05; AC:AC-RPT-05-06.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request có 3 notes,2 questions và 5 history events.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-05.
2. Thiết lập chính xác dữ liệu: Một request có 3 notes,2 questions và 5 history events.
3. Thực hiện GET /api/reports/05 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Expected:Chỉ tính request đó một lần, không nhân bản bởi dữ liệu con.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-01 — Đối chiếu phép tính

FR:FR-RPT-06; AC:AC-RPT-06-01.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Có điểm 5 và 3; yêu cầu chưa đánh giá không tính.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: Có điểm 5 và 3; yêu cầu chưa đánh giá không tính.
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.

Expected:Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-02 — Bộ lọc không hợp lệ

FR:FR-RPT-06; AC:AC-RPT-06-02.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:from_date sau to_date hoặc đơn vị không tồn tại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: from_date sau to_date hoặc đơn vị không tồn tại.
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Expected:Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-03 — Vượt quyền báo cáo

FR:FR-RPT-06; AC:AC-RPT-06-03.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Expected:Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-04 — Chưa có phản hồi hoặc đổi điểm

FR:FR-RPT-06; AC:AC-RPT-06-04.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Chưa có phản hồi hoặc đổi điểm

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: Chưa có phản hồi hoặc đổi điểm
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.

Expected:Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-05 — Ranh giới kỳ

FR:FR-RPT-06; AC:AC-RPT-06-05.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Hai record có feedback.updated_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: Hai record có feedback.updated_at lần lượt đúng00:00 from và đúng00:00 ngày sau to theo giờ Việt Nam.
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Expected:Chỉ record tại biên đầu được tính; biên cuối bị loại. Không lệch múi giờ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-06 — Join không nhân bản

FR:FR-RPT-06; AC:AC-RPT-06-06.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Một request có 3 notes,2 questions và 5 history events.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: Một request có 3 notes,2 questions và 5 history events.
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Chỉ tính feedback hiện hành của request đó một lần, không nhân bản bởi dữ liệu con.

Expected:Chỉ tính feedback hiện hành của request đó một lần, không nhân bản bởi dữ liệu con.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-RPT-06-07 — Sửa điểm và kỳ cập nhật

FR:FR-RPT-06; AC:AC-RPT-06-07.

Preconditions:Có quyền quản lý trong phạm vi báo cáo được cấu hình.

Dữ liệu:Feedback4 tại kỳA đổi thành3 tại kỳB không trùng A.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-RPT-06.
2. Thiết lập chính xác dữ liệu: Feedback4 tại kỳA đổi thành3 tại kỳB không trùng A.
3. Thực hiện GET /api/reports/06 theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Cùng feedback_id/request_id; kỳA không còn dòng này, kỳB có điểm3; mẫu toàn thời gian vẫn1.

Expected:Cùng feedback_id/request_id; kỳA không còn dòng này, kỳB có điểm3; mẫu toàn thời gian vẫn1.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-01-01 — Đúng tài khoản

FR:FR-IAM-01; AC:AC-IAM-01-01.

Preconditions:Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

Dữ liệu:Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-01.
2. Thiết lập chính xác dữ liệu: Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập.
3. Thực hiện POST /api/auth/login theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.

Expected:Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-01-02 — Sai thông tin

FR:FR-IAM-01; AC:AC-IAM-01-02.

Preconditions:Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

Dữ liệu:Sai mật khẩu hoặc email không có.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-01.
2. Thiết lập chính xác dữ liệu: Sai mật khẩu hoặc email không có.
3. Thực hiện POST /api/auth/login theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Thông báo chung; không tạo phiên được phép nghiệp vụ.

Expected:Thông báo chung; không tạo phiên được phép nghiệp vụ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-01-03 — Chưa xác thực

FR:FR-IAM-01; AC:AC-IAM-01-03.

Preconditions:Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

Dữ liệu:Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-01.
2. Thiết lập chính xác dữ liệu: Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên.
3. Thực hiện POST /api/auth/login theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.

Expected:Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-01-04 — Cố định phiên / hết hạn

FR:FR-IAM-01; AC:AC-IAM-01-04.

Preconditions:Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

Dữ liệu:Dùng mã phiên trước đăng nhập hoặc phiên hết hạn idle 30 phút, absolute 8 giờ.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-01.
2. Thiết lập chính xác dữ liệu: Dùng mã phiên trước đăng nhập hoặc phiên hết hạn idle 30 phút, absolute 8 giờ.
3. Thực hiện POST /api/auth/login theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.

Expected:Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-01-05 — Tài khoản inactive

FR:FR-IAM-01; AC:AC-IAM-01-05.

Preconditions:Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

Dữ liệu:Đúng password nhưng active=false.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-01.
2. Thiết lập chính xác dữ liệu: Đúng password nhưng active=false.
3. Thực hiện POST /api/auth/login theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 401 INVALID_CREDENTIALS như email không có; không cấp session.

Expected:401 INVALID_CREDENTIALS như email không có; không cấp session.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-01-06 — Giới hạn thử sai

FR:FR-IAM-01; AC:AC-IAM-01-06.

Preconditions:Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ.

Dữ liệu:5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-01.
2. Thiết lập chính xác dữ liệu: 5 lần sai trong15 phút, sau đó thử lần6 cùng email+IP.
3. Thực hiện POST /api/auth/login theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn.

Expected:Lần6 trả 429 có thời gian thử lại; hết cửa sổ mới được thử, không khóa tài khoản vĩnh viễn.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-02-01 — Đăng xuất đúng

FR:FR-IAM-02; AC:AC-IAM-02-01.

Preconditions:Đã đăng nhập hợp lệ.

Dữ liệu:Đăng nhập rồi chọn Đăng xuất.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-02.
2. Thiết lập chính xác dữ liệu: Đăng nhập rồi chọn Đăng xuất.
3. Thực hiện POST /api/auth/logout theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Trở về đăng nhập; phiên máy chủ bị vô hiệu.

Expected:Trở về đăng nhập; phiên máy chủ bị vô hiệu.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-02-02 — Không có phiên

FR:FR-IAM-02; AC:AC-IAM-02-02.

Preconditions:Đã đăng nhập hợp lệ.

Dữ liệu:Gọi đăng xuất khi đã thoát.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-02.
2. Thiết lập chính xác dữ liệu: Gọi đăng xuất khi đã thoát.
3. Thực hiện POST /api/auth/logout theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.

Expected:Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-02-03 — Dùng lại phiên

FR:FR-IAM-02; AC:AC-IAM-02-03.

Preconditions:Đã đăng nhập hợp lệ.

Dữ liệu:Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-02.
2. Thiết lập chính xác dữ liệu: Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất.
3. Thực hiện POST /api/auth/logout theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Bị chặn; dữ liệu không thay đổi.

Expected:Bị chặn; dữ liệu không thay đổi.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-02-04 — Back / đăng nhập lại

FR:FR-IAM-02; AC:AC-IAM-02-04.

Preconditions:Đã đăng nhập hợp lệ.

Dữ liệu:Back trang trước rồi đăng nhập lại.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-02.
2. Thiết lập chính xác dữ liệu: Back trang trước rồi đăng nhập lại.
3. Thực hiện POST /api/auth/logout theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.

Expected:Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-03-01 — Quyền phù hợp

FR:FR-IAM-03; AC:AC-IAM-03-01.

Preconditions:Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

Dữ liệu:SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-03.
2. Thiết lập chính xác dữ liệu: SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi.
3. Thực hiện Middleware Tất cả protected endpoints theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.

Expected:Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-03-02 — Vai trò giả

FR:FR-IAM-03; AC:AC-IAM-03-02.

Preconditions:Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

Dữ liệu:Gửi role=manager hoặc scope=all trong dữ liệu.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-03.
2. Thiết lập chính xác dữ liệu: Gửi role=manager hoặc scope=all trong dữ liệu.
3. Thực hiện Middleware Tất cả protected endpoints theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.

Expected:Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-03-03 — Đối tượng của người khác

FR:FR-IAM-03; AC:AC-IAM-03-03.

Preconditions:Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

Dữ liệu:SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-03.
2. Thiết lập chính xác dữ liệu: SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi.
3. Thực hiện Middleware Tất cả protected endpoints theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.

Expected:Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-03-04 — Thay trách nhiệm / dữ liệu nội bộ

FR:FR-IAM-03; AC:AC-IAM-03-04.

Preconditions:Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

Dữ liệu:Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-03.
2. Thiết lập chính xác dữ liệu: Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp.
3. Thực hiện Middleware Tất cả protected endpoints theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.

Expected:NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-03-05 — CSRF thiếu/sai

FR:FR-IAM-03; AC:AC-IAM-03-05.

Preconditions:Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

Dữ liệu:Đăng nhập hợp lệ rồi gửi POST protected không CSRF hoặc CSRF của phiên khác.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-03.
2. Thiết lập chính xác dữ liệu: Đăng nhập hợp lệ rồi gửi POST protected không CSRF hoặc CSRF của phiên khác.
3. Thực hiện Middleware Tất cả protected endpoints theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: 403 CSRF_INVALID; không ghi dữ liệu. GET chỉ đọc không yêu cầu CSRF.

Expected:403 CSRF_INVALID; không ghi dữ liệu. GET chỉ đọc không yêu cầu CSRF.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:

## TC-IAM-03-06 — Aggregate vượt scope

FR:FR-IAM-03; AC:AC-IAM-03-06.

Preconditions:Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ.

Dữ liệu:QL-A chỉ PB-A; gọi report PB-B hoặc scope=all.

1. Reset seed/fixture của case; đặt trạng thái và actor đúng Preconditions của FR-IAM-03.
2. Thiết lập chính xác dữ liệu: QL-A chỉ PB-A; gọi report PB-B hoặc scope=all.
3. Thực hiện Middleware Tất cả protected endpoints theo luồng FR. Case trái quyền gọi API trực tiếp, không chỉ kiểm nút UI.
4. Đối chiếu HTTP/response và đọc lại UI; case ghi kiểm số bản ghi, version, history trước/sau. Case report đối chiếu tập nguồn theo tiêu chí kỳ của FR.
5. Ghi actual, evidence, build, date, result. Expected: PB-B403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A.

Expected:PB-B403; scope=all không mở rộng quyền; không có source/số liệu ngoài PB-A.

Result:Not Run; Actual:; Build:; Evidence:; Bug:; Retest:
