# 96 test case chức năng

Các dữ liệu tên/mã đều giả lập. Không đánh Pass trước khi chạy.

## TC-STU-01-01 — Đủ trường, loại hợp lệ

- **FR / AC:** FR-STU-01 / AC-STU-01-01.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Dữ liệu mẫu: loại Học vụ, tiêu đề Xin xác nhận sinh viên, mô tả Xin hướng dẫn thủ tục.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện gửi yêu cầu hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Tạo đúng một yêu cầu có mã duy nhất, chủ từ phiên, Đã tiếp nhận; hàng chờ thấy cùng mã.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-01-02 — Thiếu nội dung

- **FR / AC:** FR-STU-01 / AC-STU-01-02.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Tiêu đề chỉ có khoảng trắng hoặc mô tả trống.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện gửi yêu cầu hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo tại trường; không tạo yêu cầu hay lịch sử một phần.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-01-03 — Giả mạo chủ yêu cầu

- **FR / AC:** FR-STU-01 / AC-STU-01-03.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Phiên SV-A gửi thêm student_id=SV-B.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện gửi yêu cầu hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Chủ vẫn là SV-A; không chấp nhận student_id từ trình duyệt.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-01-04 — Gửi lặp

- **FR / AC:** FR-STU-01 / AC-STU-01-04.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Sinh viên đã đăng nhập; danh mục vấn đề giả lập đã được khởi tạo. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi lại cùng submission_key hai lần.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện gửi yêu cầu hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Trả cùng mã; tổng số yêu cầu tăng một, không hai.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-02-01 — Có yêu cầu và bộ lọc

- **FR / AC:** FR-STU-02 / AC-STU-02-01.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV-A có hai yêu cầu Đã tiếp nhận và Đang xử lý; lọc Đang xử lý.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem danh sách yêu cầu của tôi; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Chỉ trả yêu cầu Đang xử lý của SV-A; tổng số và phân trang dùng cùng điều kiện.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-02-02 — Tham số sai

- **FR / AC:** FR-STU-02 / AC-STU-02-02.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** page=0 hoặc trạng thái không thuộc danh sách.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem danh sách yêu cầu của tôi; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo bộ lọc không hợp lệ; không trả tập dữ liệu vượt quyền.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-02-03 — Lọc theo người khác

- **FR / AC:** FR-STU-02 / AC-STU-02-03.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV-A sửa tham số student_id thành SV-B.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem danh sách yêu cầu của tôi; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không trả bất kỳ yêu cầu của SV-B.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-02-04 — Danh sách rỗng

- **FR / AC:** FR-STU-02 / AC-STU-02-04.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên sinh viên hợp lệ; có thể chưa có yêu cầu. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Tài khoản SV-C chưa có yêu cầu.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem danh sách yêu cầu của tôi; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Hiển thị Chưa có yêu cầu và liên kết gửi yêu cầu; không báo lỗi hệ thống.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-03-01 — Tiến độ đã cập nhật

- **FR / AC:** FR-STU-03 / AC-STU-03-01.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Yêu cầu được phân công rồi chuyển Đang xử lý.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem chi tiết và tiến độ yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Chi tiết và danh sách cùng hiển thị trạng thái/người phụ trách hiện hành sau tải lại.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-03-02 — Mã không tồn tại

- **FR / AC:** FR-STU-03 / AC-STU-03-02.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Mã yêu cầu không có trong bộ dữ liệu.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem chi tiết và tiến độ yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Hiển thị không tìm thấy; không tạo hoặc sửa dữ liệu.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-03-03 — Truy cập ngang

- **FR / AC:** FR-STU-03 / AC-STU-03-03.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV-B mở URL yêu cầu của SV-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem chi tiết và tiến độ yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không trả tiêu đề, mô tả, lịch sử hoặc tài liệu của SV-A.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-03-04 — Chưa phân công / chờ bổ sung

- **FR / AC:** FR-STU-03 / AC-STU-03-04.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu tồn tại và thuộc sinh viên đang đăng nhập. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Xem yêu cầu mới, sau đó xem yêu cầu Chờ bổ sung.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem chi tiết và tiến độ yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Yêu cầu mới ghi Chưa phân công; yêu cầu chờ bổ sung ghi rõ thông tin cần cung cấp, không lộ ghi chú nội bộ.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-04-01 — Trả lời hợp lệ

- **FR / AC:** FR-STU-04 / AC-STU-04-01.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Câu hỏi đang mở: cần mã lớp; trả lời Mã lớp TEST-01.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bổ sung thông tin được yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Cùng mã yêu cầu, chủ và người phụ trách; câu hỏi có trả lời; trạng thái Đang xử lý.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-04-02 — Nội dung trống

- **FR / AC:** FR-STU-04 / AC-STU-04-02.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Trả lời chỉ có khoảng trắng.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bổ sung thông tin được yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi; vẫn Chờ bổ sung; chưa đánh dấu câu hỏi đã trả lời.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-04-03 — Sai chủ

- **FR / AC:** FR-STU-04 / AC-STU-04-03.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV-B gửi câu trả lời cho yêu cầu SV-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bổ sung thông tin được yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; nội dung và trạng thái không đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-04-04 — Gửi từ màn hình cũ

- **FR / AC:** FR-STU-04 / AC-STU-04-04.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Chờ bổ sung; có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Câu hỏi đã được trả lời hoặc phiên bản hồ sơ đã đổi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bổ sung thông tin được yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo tải lại/đã xử lý; không tạo hai câu trả lời hay ghi đè cập nhật mới.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-05-01 — Phản hồi hợp lệ

- **FR / AC:** FR-STU-05 / AC-STU-05-01.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Yêu cầu đã có kết quả; điểm 4, nhận xét Đã được hướng dẫn.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phản hồi kết quả hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Lưu phản hồi gắn đúng yêu cầu và sinh viên; hiển thị lại điểm 4.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-05-02 — Điểm sai / chưa có kết quả

- **FR / AC:** FR-STU-05 / AC-STU-05-02.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Điểm 0, 6, 2.5 hoặc yêu cầu Đang xử lý.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phản hồi kết quả hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không có phản hồi mới để tính báo cáo.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-05-03 — Không phải chủ

- **FR / AC:** FR-STU-05 / AC-STU-05-03.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV-B gửi đánh giá cho yêu cầu SV-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phản hồi kết quả hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; phản hồi của SV-A không bị sửa.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-STU-05-04 — Cập nhật phản hồi

- **FR / AC:** FR-STU-05 / AC-STU-05-04.
- **Actor:** Sinh viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đúng chủ yêu cầu; trạng thái Đã giải quyết hoặc Đã đóng; kết quả xử lý đã có. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV-A đổi điểm từ 4 thành 3.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phản hồi kết quả hỗ trợ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Một phản hồi hiện hành với điểm 3; số lượt phản hồi không tăng; trạng thái xử lý không tự đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-01-01 — Yêu cầu mới

- **FR / AC:** FR-DSP-01 / AC-DSP-01-01.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền điều phối trong phạm vi được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Tạo yêu cầu Chưa xác định bằng STU-01.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hàng chờ điều phối; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-01-02 — Lọc sai

- **FR / AC:** FR-DSP-01 / AC-DSP-01-02.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền điều phối trong phạm vi được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** category_id không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hàng chờ điều phối; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo tham số sai; không áp dụng loại khác âm thầm.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-01-03 — Vai trò không được phép

- **FR / AC:** FR-DSP-01 / AC-DSP-01-03.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền điều phối trong phạm vi được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi trang/điểm truy cập hàng chờ.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hàng chờ điều phối; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không lộ danh sách toàn trường.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-01-04 — Sau phân công

- **FR / AC:** FR-DSP-01 / AC-DSP-01-04.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền điều phối trong phạm vi được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Phân công yêu cầu rồi lọc unassigned=true.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hàng chờ điều phối; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-02-01 — Có lịch sử

- **FR / AC:** FR-DSP-02 / AC-DSP-02-01.
- **Actor:** Điều phối viên / Nhân viên xử lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền trên yêu cầu theo vai trò và phạm vi công việc. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Yêu cầu đã tiếp nhận, phân loại, phân công.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hồ sơ xử lý và lịch sử nghiệp vụ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-02-02 — Mã không có

- **FR / AC:** FR-DSP-02 / AC-DSP-02-02.
- **Actor:** Điều phối viên / Nhân viên xử lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền trên yêu cầu theo vai trò và phạm vi công việc. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Mở mã không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hồ sơ xử lý và lịch sử nghiệp vụ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không tìm thấy; không sinh dữ liệu.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-02-03 — Ngoài phạm vi

- **FR / AC:** FR-DSP-02 / AC-DSP-02-03.
- **Actor:** Điều phối viên / Nhân viên xử lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền trên yêu cầu theo vai trò và phạm vi công việc. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hồ sơ xử lý và lịch sử nghiệp vụ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối, không trả nội dung sinh viên.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-02-04 — Ghi chú nội bộ

- **FR / AC:** FR-DSP-02 / AC-DSP-02-04.
- **Actor:** Điều phối viên / Nhân viên xử lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền trên yêu cầu theo vai trò và phạm vi công việc. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện xem hồ sơ xử lý và lịch sử nghiệp vụ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-03-01 — Gán loại

- **FR / AC:** FR-DSP-03 / AC-DSP-03-01.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Yêu cầu Chưa xác định được gán Học vụ.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân loại yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-03-02 — Danh mục sai

- **FR / AC:** FR-DSP-03 / AC-DSP-03-02.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi loại đã ngừng dùng hoặc ID không có.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân loại yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; loại và lịch sử không thay đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-03-03 — Không có quyền

- **FR / AC:** FR-DSP-03 / AC-DSP-03-03.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Nhân viên thường hoặc sinh viên tự đổi loại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân loại yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; loại hiện hành giữ nguyên.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-03-04 — Đã phân công / thao tác đồng thời

- **FR / AC:** FR-DSP-03 / AC-DSP-03-04.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân loại yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-04-01 — Phân công lần đầu

- **FR / AC:** FR-DSP-04 / AC-DSP-04-01.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Giao yêu cầu cho NV-A thuộc PB-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân công trách nhiệm xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-04-02 — Người không thuộc đơn vị

- **FR / AC:** FR-DSP-04 / AC-DSP-04-02.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Chọn PB-A với nhân viên PB-B.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân công trách nhiệm xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không tạo trách nhiệm không nhất quán.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-04-03 — Tự chiếm yêu cầu

- **FR / AC:** FR-DSP-04 / AC-DSP-04-03.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Nhân viên không có quyền điều phối gửi assignee_id của mình.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân công trách nhiệm xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không thay trách nhiệm.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-04-04 — Đổi trách nhiệm đồng thời

- **FR / AC:** FR-DSP-04 / AC-DSP-04-04.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện phân công trách nhiệm xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-05-01 — Hạn hợp lệ

- **FR / AC:** FR-DSP-05 / AC-DSP-05-01.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc và thuộc phạm vi điều phối. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Hạn hai ngày sau created_at.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đặt hạn xử lý dự kiến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-05-02 — Hạn trước khi tạo

- **FR / AC:** FR-DSP-05 / AC-DSP-05-02.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc và thuộc phạm vi điều phối. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** due_at nhỏ hơn created_at.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đặt hạn xử lý dự kiến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; hạn cũ giữ nguyên.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-05-03 — Sai quyền

- **FR / AC:** FR-DSP-05 / AC-DSP-05-03.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc và thuộc phạm vi điều phối. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên hoặc nhân viên không điều phối sửa hạn.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đặt hạn xử lý dự kiến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không thay hạn.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-05-04 — Không hạn / đúng ranh giới

- **FR / AC:** FR-DSP-05 / AC-DSP-05-04.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Yêu cầu chưa kết thúc và thuộc phạm vi điều phối. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đặt hạn xử lý dự kiến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-06-01 — Bắt đầu đúng

- **FR / AC:** FR-DSP-06 / AC-DSP-06-01.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bắt đầu xử lý yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-06-02 — Trạng thái sai

- **FR / AC:** FR-DSP-06 / AC-DSP-06-02.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Bắt đầu hồ sơ Đã đóng.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bắt đầu xử lý yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không thay trạng thái và thời điểm.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-06-03 — Người khác thao tác

- **FR / AC:** FR-DSP-06 / AC-DSP-06-03.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** NV-B bắt đầu hồ sơ giao NV-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bắt đầu xử lý yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; trách nhiệm giữ nguyên.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-06-04 — Bấm hai lần

- **FR / AC:** FR-DSP-06 / AC-DSP-06-04.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi lặp lần bắt đầu.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện bắt đầu xử lý yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-07-01 — Câu hỏi hợp lệ

- **FR / AC:** FR-DSP-07 / AC-DSP-07-01.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Đang xử lý, hỏi Vui lòng cung cấp mã lớp.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện yêu cầu sinh viên bổ sung thông tin; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-07-02 — Câu hỏi rỗng

- **FR / AC:** FR-DSP-07 / AC-DSP-07-02.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Nội dung chỉ có khoảng trắng.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện yêu cầu sinh viên bổ sung thông tin; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không lưu câu hỏi hoặc chuyển trạng thái.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-07-03 — Ngoài trách nhiệm

- **FR / AC:** FR-DSP-07 / AC-DSP-07-03.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** NV-B yêu cầu bổ sung hồ sơ giao NV-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện yêu cầu sinh viên bổ sung thông tin; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không lộ thêm dữ liệu.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-07-04 — Đã có câu hỏi mở

- **FR / AC:** FR-DSP-07 / AC-DSP-07-04.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi lần thứ hai khi Chờ bổ sung.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện yêu cầu sinh viên bổ sung thông tin; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối/nhắc câu hỏi hiện hành; không có hai câu hỏi mở.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-08-01 — Tiến độ công khai

- **FR / AC:** FR-DSP-08 / AC-DSP-08-01.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý hoặc Chờ bổ sung; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Ghi Đang kiểm tra thủ tục, visibility=public.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi cập nhật tiến độ xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-08-02 — Giá trị sai

- **FR / AC:** FR-DSP-08 / AC-DSP-08-02.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý hoặc Chờ bổ sung; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Nội dung trống hoặc visibility=secret.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi cập nhật tiến độ xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không thêm lịch sử.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-08-03 — Mất trách nhiệm

- **FR / AC:** FR-DSP-08 / AC-DSP-08-03.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý hoặc Chờ bổ sung; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi cập nhật tiến độ xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không ghi nội dung mới.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-08-04 — Tiến độ nội bộ

- **FR / AC:** FR-DSP-08 / AC-DSP-08-04.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý hoặc Chờ bổ sung; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Ghi ghi chú internal có dấu TEST-INTERNAL.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi cập nhật tiến độ xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-09-01 — Có kết quả

- **FR / AC:** FR-DSP-09 / AC-DSP-09-01.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi kết quả giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-09-02 — Còn thiếu thông tin / kết quả trống

- **FR / AC:** FR-DSP-09 / AC-DSP-09-02.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Chờ bổ sung hoặc resolution rỗng.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi kết quả giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-09-03 — Sai người xử lý

- **FR / AC:** FR-DSP-09 / AC-DSP-09-03.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** NV-B ghi kết quả cho hồ sơ NV-A.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi kết quả giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không thay kết quả.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-09-04 — Gửi lại / dữ liệu cũ

- **FR / AC:** FR-DSP-09 / AC-DSP-09-04.
- **Actor:** Nhân viên được giao; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi lại cùng thao tác hoặc dùng record_version cũ.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện ghi kết quả giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-10-01 — Đóng đúng

- **FR / AC:** FR-DSP-10 / AC-DSP-10-01.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Điều phối viên đóng yêu cầu Đã giải quyết có kết quả.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đóng yêu cầu đã giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-10-02 — Đóng trước khi giải quyết

- **FR / AC:** FR-DSP-10 / AC-DSP-10-02.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Hồ sơ Đang xử lý chưa có kết quả.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đóng yêu cầu đã giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không bỏ qua bước giải quyết.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-10-03 — Sai vai trò

- **FR / AC:** FR-DSP-10 / AC-DSP-10-03.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên hoặc nhân viên thường gọi thao tác đóng.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đóng yêu cầu đã giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; trạng thái không đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-DSP-10-04 — Đóng lặp

- **FR / AC:** FR-DSP-10 / AC-DSP-10-04.
- **Actor:** Điều phối viên; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi lại thao tác đóng; sau đó thử ghi tiến độ.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đóng yêu cầu đã giải quyết; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-01-01 — Đối chiếu phép tính

- **FR / AC:** FR-RPT-01 / AC-RPT-01-01.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Tạo 10 yêu cầu: 2 mỗi trạng thái.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp trạng thái yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Mỗi nhóm bằng 2, tổng bằng 10.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-01-02 — Bộ lọc không hợp lệ

- **FR / AC:** FR-RPT-01 / AC-RPT-01-02.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** from_date sau to_date hoặc đơn vị không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp trạng thái yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-01-03 — Vượt quyền báo cáo

- **FR / AC:** FR-RPT-01 / AC-RPT-01-03.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp trạng thái yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-01-04 — Không có yêu cầu

- **FR / AC:** FR-RPT-01 / AC-RPT-01-04.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Không có yêu cầu
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp trạng thái yêu cầu; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-02-01 — Đối chiếu phép tính

- **FR / AC:** FR-RPT-02 / AC-RPT-02-01.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Có một yêu cầu mở quá hạn, một đúng hạn, một không hạn, một Đã giải quyết quá hạn.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện theo dõi yêu cầu quá hạn; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Chỉ yêu cầu mở quá hạn xuất hiện.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-02-02 — Bộ lọc không hợp lệ

- **FR / AC:** FR-RPT-02 / AC-RPT-02-02.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** from_date sau to_date hoặc đơn vị không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện theo dõi yêu cầu quá hạn; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-02-03 — Vượt quyền báo cáo

- **FR / AC:** FR-RPT-02 / AC-RPT-02-03.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện theo dõi yêu cầu quá hạn; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-02-04 — now bằng due_at

- **FR / AC:** FR-RPT-02 / AC-RPT-02-04.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** now bằng due_at
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện theo dõi yêu cầu quá hạn; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Yêu cầu chưa bị tính quá hạn tại đúng ranh giới.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-03-01 — Đối chiếu phép tính

- **FR / AC:** FR-RPT-03 / AC-RPT-03-01.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Hai hồ sơ có thời gian 2 giờ và 4 giờ; một hồ sơ còn mở.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê thời gian xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-03-02 — Bộ lọc không hợp lệ

- **FR / AC:** FR-RPT-03 / AC-RPT-03-02.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** from_date sau to_date hoặc đơn vị không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê thời gian xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-03-03 — Vượt quyền báo cáo

- **FR / AC:** FR-RPT-03 / AC-RPT-03-03.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê thời gian xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-03-04 — Không hồ sơ đã giải quyết

- **FR / AC:** FR-RPT-03 / AC-RPT-03-04.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Không hồ sơ đã giải quyết
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê thời gian xử lý; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-04-01 — Đối chiếu phép tính

- **FR / AC:** FR-RPT-04 / AC-RPT-04-01.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Ba Học vụ, hai CNTT và một Chưa xác định.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê vấn đề phổ biến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-04-02 — Bộ lọc không hợp lệ

- **FR / AC:** FR-RPT-04 / AC-RPT-04-02.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** from_date sau to_date hoặc đơn vị không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê vấn đề phổ biến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-04-03 — Vượt quyền báo cáo

- **FR / AC:** FR-RPT-04 / AC-RPT-04-03.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê vấn đề phổ biến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-04-04 — Hai nhóm bằng số lượng

- **FR / AC:** FR-RPT-04 / AC-RPT-04-04.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Hai nhóm bằng số lượng
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê vấn đề phổ biến; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Thứ tự phụ theo tên nhóm ổn định; không bỏ nhóm chưa xác định.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-05-01 — Đối chiếu phép tính

- **FR / AC:** FR-RPT-05 / AC-RPT-05-01.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** PB-A có hai mở, một kết thúc; PB-B có một mở; một chưa phân công.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê khối lượng theo phòng ban; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** PB-A tổng 3/mở 2; PB-B 1/1; chưa phân công 1/1.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-05-02 — Bộ lọc không hợp lệ

- **FR / AC:** FR-RPT-05 / AC-RPT-05-02.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** from_date sau to_date hoặc đơn vị không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê khối lượng theo phòng ban; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-05-03 — Vượt quyền báo cáo

- **FR / AC:** FR-RPT-05 / AC-RPT-05-03.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê khối lượng theo phòng ban; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-05-04 — Chuyển một hồ sơ PB-A sang PB-B

- **FR / AC:** FR-RPT-05 / AC-RPT-05-04.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Chuyển một hồ sơ PB-A sang PB-B
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện thống kê khối lượng theo phòng ban; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Sau chuyển, tổng toàn hệ thống giữ nguyên; chỉ nhóm hiện hành tăng/giảm tương ứng.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-06-01 — Đối chiếu phép tính

- **FR / AC:** FR-RPT-06 / AC-RPT-06-01.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Có điểm 5 và 3; yêu cầu chưa đánh giá không tính.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp mức hài lòng; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-06-02 — Bộ lọc không hợp lệ

- **FR / AC:** FR-RPT-06 / AC-RPT-06-02.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** from_date sau to_date hoặc đơn vị không tồn tại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp mức hài lòng; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-06-03 — Vượt quyền báo cáo

- **FR / AC:** FR-RPT-06 / AC-RPT-06-03.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp mức hài lòng; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-RPT-06-04 — Chưa có phản hồi hoặc đổi điểm

- **FR / AC:** FR-RPT-06 / AC-RPT-06-04.
- **Actor:** Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có quyền quản lý trong phạm vi báo cáo được cấu hình. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Chưa có phản hồi hoặc đổi điểm
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện tổng hợp mức hài lòng; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-01-01 — Đúng tài khoản

- **FR / AC:** FR-IAM-01 / AC-IAM-01-01.
- **Actor:** Sinh viên / Điều phối viên / Nhân viên / Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Lần lượt đăng nhập bốn vai trò bằng tài khoản giả lập.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng nhập; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Có phiên mới cho đúng người, đúng khu vực; không sử dụng vai trò do trình duyệt tự khai.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-01-02 — Sai thông tin

- **FR / AC:** FR-IAM-01 / AC-IAM-01-02.
- **Actor:** Sinh viên / Điều phối viên / Nhân viên / Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Sai mật khẩu hoặc email không có.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng nhập; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Thông báo chung; không tạo phiên được phép nghiệp vụ.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-01-03 — Chưa xác thực

- **FR / AC:** FR-IAM-01 / AC-IAM-01-03.
- **Actor:** Sinh viên / Điều phối viên / Nhân viên / Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Mở trang nghiệp vụ hoặc gọi API trực tiếp không phiên.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng nhập; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Yêu cầu xác thực; không trả dữ liệu nghiệp vụ.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-01-04 — Cố định phiên / hết hạn

- **FR / AC:** FR-IAM-01 / AC-IAM-01-04.
- **Actor:** Sinh viên / Điều phối viên / Nhân viên / Quản lý; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Tài khoản giả lập đã khởi tạo với mật khẩu băm; chưa có phiên hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Dùng mã phiên trước đăng nhập hoặc phiên hết hạn CFG-08.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng nhập; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Mã phiên cũ không có quyền; phiên hết hạn không ghi nghiệp vụ; yêu cầu đăng nhập lại.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-02-01 — Đăng xuất đúng

- **FR / AC:** FR-IAM-02 / AC-IAM-02-01.
- **Actor:** Mọi vai trò đã đăng nhập; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã đăng nhập hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Đăng nhập rồi chọn Đăng xuất.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng xuất; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Trở về đăng nhập; phiên máy chủ bị vô hiệu.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-02-02 — Không có phiên

- **FR / AC:** FR-IAM-02 / AC-IAM-02-02.
- **Actor:** Mọi vai trò đã đăng nhập; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã đăng nhập hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gọi đăng xuất khi đã thoát.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng xuất; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Kết quả an toàn về đăng nhập, không lỗi hệ thống hay ảnh hưởng tài khoản khác.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-02-03 — Dùng lại phiên

- **FR / AC:** FR-IAM-02 / AC-IAM-02-03.
- **Actor:** Mọi vai trò đã đăng nhập; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã đăng nhập hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gọi cập nhật nghiệp vụ bằng phiên đã đăng xuất.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng xuất; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Bị chặn; dữ liệu không thay đổi.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-02-04 — Back / đăng nhập lại

- **FR / AC:** FR-IAM-02 / AC-IAM-02-04.
- **Actor:** Mọi vai trò đã đăng nhập; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Đã đăng nhập hợp lệ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Back trang trước rồi đăng nhập lại.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện đăng xuất; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Back không khôi phục quyền; đăng nhập mới tạo phiên mới hợp lệ.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-03-01 — Quyền phù hợp

- **FR / AC:** FR-IAM-03 / AC-IAM-03-01.
- **Actor:** Máy chủ cho mọi vai trò; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV đọc hồ sơ mình; NV đọc/sửa được giao; điều phối phân công trong phạm vi; quản lý xem báo cáo trong phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện kiểm soát quyền theo vai trò và hồ sơ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Mỗi hành động hợp lệ thành công theo ma trận; vai trò không được cấp không có hành động đó.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-03-02 — Vai trò giả

- **FR / AC:** FR-IAM-03 / AC-IAM-03-02.
- **Actor:** Máy chủ cho mọi vai trò; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Gửi role=manager hoặc scope=all trong dữ liệu.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện kiểm soát quyền theo vai trò và hồ sơ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Máy chủ bỏ qua; không nâng quyền từ dữ liệu trình duyệt.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-03-03 — Đối tượng của người khác

- **FR / AC:** FR-IAM-03 / AC-IAM-03-03.
- **Actor:** Máy chủ cho mọi vai trò; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** SV thay request_id; NV đọc ngoài trách nhiệm; quản lý đổi department ngoài phạm vi.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện kiểm soát quyền theo vai trò và hồ sơ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** Đều bị chặn; không lộ nội dung, số liệu hoặc tài liệu liên quan.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.

## TC-IAM-03-04 — Thay trách nhiệm / dữ liệu nội bộ

- **FR / AC:** FR-IAM-03 / AC-IAM-03-04.
- **Actor:** Máy chủ cho mọi vai trò; nếu case vượt quyền, dùng tài khoản trái quyền đã nêu.
- **Tiền điều kiện:** Có phiên; vai trò và phạm vi mẫu được quản lý phía máy chủ. Trừ điều kiện bị cố ý vi phạm trong case.
- **Dữ liệu:** Giao lại hồ sơ, thử từ phiên NV cũ; SV đọc nội bộ trực tiếp.
- **Bước thực hiện:**
  1. Reset seed phù hợp; đọc trạng thái/giá trị trước thao tác.
  2. Mở giao diện của FR; nhập/áp tham số trong dữ liệu case. Với case quyền, gọi trực tiếp điểm truy cập tương ứng bằng phiên trái quyền để tránh chỉ thử nút ẩn.
  3. Thực hiện kiểm soát quyền theo vai trò và hồ sơ; với gửi lặp/đồng thời, lặp theo dữ liệu case.
  4. Đọc lại giao diện/API và đối chiếu bản ghi/lịch sử hoặc tập nguồn báo cáo.
- **Expected:** NV cũ không sửa được; SV không nhận ghi chú nội bộ trong HTML/JSON hay tài liệu nếu sau này được bổ sung.
- **Actual / Result:** Chưa thực thi / Not Run.
- **Evidence:** Chưa có; khi chạy ghi ảnh/response/query đã loại secret, thời điểm và build.
- **Dọn dữ liệu:** Restore fixture của case; không dùng kết quả case trước làm điều kiện ẩn.
