# PRD module: Quản lý và báo cáo

Bản tổng hợp để review; file từng FR và requirements.json giữ cùng nội dung.

# [FR-RPT-01] Tổng hợp trạng thái yêu cầu

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Đếm yêu cầu theo trạng thái để theo dõi tổng thể.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| status | Nguồn đọc | Mỗi yêu cầu thuộc đúng một trong năm trạng thái; tổng nhóm bằng tổng tập lọc. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- Không có yêu cầu: Không có yêu cầu → Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-01-01 | Đối chiếu phép tính | Mỗi nhóm bằng 2, tổng bằng 10. |
| AC-RPT-01-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-01-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-01-04 | Không có yêu cầu | Mỗi nhóm bằng 0; không chia cho 0; hiển thị bộ lọc đang áp dụng. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-01-01 | AC-RPT-01-01 | Tạo 10 yêu cầu: 2 mỗi trạng thái. |
| TC-RPT-01-02 | AC-RPT-01-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-01-03 | AC-RPT-01-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-01-04 | AC-RPT-01-04 | Không có yêu cầu |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Mỗi nhóm bằng 2, tổng bằng 10.

Master dùng parent `FR-RPT-01 | Tổng hợp trạng thái yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-DSP-10


---

# [FR-RPT-02] Theo dõi yêu cầu quá hạn

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Nhận biết yêu cầu còn mở đã vượt hạn dự kiến.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| due_at / status | Nguồn đọc | Quá hạn khi now > due_at và trạng thái thuộc Đã tiếp nhận, Đang xử lý, Chờ bổ sung; không hạn không tính. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- now bằng due_at: now bằng due_at → Yêu cầu chưa bị tính quá hạn tại đúng ranh giới.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-02-01 | Đối chiếu phép tính | Chỉ yêu cầu mở quá hạn xuất hiện. |
| AC-RPT-02-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-02-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-02-04 | now bằng due_at | Yêu cầu chưa bị tính quá hạn tại đúng ranh giới. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-02-01 | AC-RPT-02-01 | Có một yêu cầu mở quá hạn, một đúng hạn, một không hạn, một Đã giải quyết quá hạn. |
| TC-RPT-02-02 | AC-RPT-02-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-02-03 | AC-RPT-02-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-02-04 | AC-RPT-02-04 | now bằng due_at |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Chỉ yêu cầu mở quá hạn xuất hiện.

Master dùng parent `FR-RPT-02 | Theo dõi yêu cầu quá hạn` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-05


---

# [FR-RPT-03] Thống kê thời gian xử lý

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Đo thời gian từ gửi đến giải quyết bằng dữ liệu mẫu.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| created_at / resolved_at | Nguồn đọc | Thời gian = resolved_at − created_at; chỉ hồ sơ có resolved_at trong kỳ; báo số mẫu và trung bình, không tự trừ thời gian chờ. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- Không hồ sơ đã giải quyết: Không hồ sơ đã giải quyết → Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-03-01 | Đối chiếu phép tính | Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại. |
| AC-RPT-03-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-03-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-03-04 | Không hồ sơ đã giải quyết | Hiện Không có dữ liệu, cỡ mẫu 0; không biểu diễn trung bình là 0 giờ. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-03-01 | AC-RPT-03-01 | Hai hồ sơ có thời gian 2 giờ và 4 giờ; một hồ sơ còn mở. |
| TC-RPT-03-02 | AC-RPT-03-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-03-03 | AC-RPT-03-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-03-04 | AC-RPT-03-04 | Không hồ sơ đã giải quyết |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Cỡ mẫu 2, trung bình 3 giờ; hồ sơ mở bị loại.

Master dùng parent `FR-RPT-03 | Thống kê thời gian xử lý` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-09


---

# [FR-RPT-04] Thống kê vấn đề phổ biến

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Biết nhóm vấn đề nào có nhiều yêu cầu.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| category_id | Nguồn đọc | Một yêu cầu tính vào loại hiện hành đúng một lần; Chưa xác định là một nhóm riêng. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- Hai nhóm bằng số lượng: Hai nhóm bằng số lượng → Thứ tự phụ theo tên nhóm ổn định; không bỏ nhóm chưa xác định.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-04-01 | Đối chiếu phép tính | Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần. |
| AC-RPT-04-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-04-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-04-04 | Hai nhóm bằng số lượng | Thứ tự phụ theo tên nhóm ổn định; không bỏ nhóm chưa xác định. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-04-01 | AC-RPT-04-01 | Ba Học vụ, hai CNTT và một Chưa xác định. |
| TC-RPT-04-02 | AC-RPT-04-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-04-03 | AC-RPT-04-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-04-04 | AC-RPT-04-04 | Hai nhóm bằng số lượng |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Các nhóm 3, 2, 1; tổng 6; sắp số lượng giảm dần.

Master dùng parent `FR-RPT-04 | Thống kê vấn đề phổ biến` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-03


---

# [FR-RPT-05] Thống kê khối lượng theo phòng ban

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Biết trách nhiệm hiện hành và lượng việc còn mở của các đơn vị.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| department_id / status | Nguồn đọc | Đếm toàn bộ và còn mở theo đơn vị hiện hành; chưa phân công là nhóm riêng; không cộng hai đơn vị sau chuyển trách nhiệm. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- Chuyển một hồ sơ PB-A sang PB-B: Chuyển một hồ sơ PB-A sang PB-B → Sau chuyển, tổng toàn hệ thống giữ nguyên; chỉ nhóm hiện hành tăng/giảm tương ứng.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-05-01 | Đối chiếu phép tính | PB-A tổng 3/mở 2; PB-B 1/1; chưa phân công 1/1. |
| AC-RPT-05-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-05-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-05-04 | Chuyển một hồ sơ PB-A sang PB-B | Sau chuyển, tổng toàn hệ thống giữ nguyên; chỉ nhóm hiện hành tăng/giảm tương ứng. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-05-01 | AC-RPT-05-01 | PB-A có hai mở, một kết thúc; PB-B có một mở; một chưa phân công. |
| TC-RPT-05-02 | AC-RPT-05-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-05-03 | AC-RPT-05-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-05-04 | AC-RPT-05-04 | Chuyển một hồ sơ PB-A sang PB-B |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

PB-A tổng 3/mở 2; PB-B 1/1; chưa phân công 1/1.

Master dùng parent `FR-RPT-05 | Thống kê khối lượng theo phòng ban` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-04


---

# [FR-RPT-06] Tổng hợp mức hài lòng

**Module:** Quản lý và báo cáo

**Nguồn phạm vi:** Proposal §2.1 — trạng thái, thời gian, vấn đề phổ biến, khối lượng và hài lòng. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Tổng hợp phản hồi kết quả để đánh giá chất lượng hỗ trợ.

## Actor

Quản lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền quản lý trong phạm vi báo cáo được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| from_date / to_date | Tùy chọn | Kỳ có from ≤ to; CFG-06 và quy tắc kỳ BR-08. |
| department / category | Tùy chọn | Lọc trong phạm vi quản lý. |
| score / feedback | Nguồn đọc | Một phản hồi hiện hành mỗi yêu cầu; điểm trung bình = tổng điểm / số phản hồi; kèm phân bố 1–5 và số phản hồi. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Quản lý chọn kỳ và phạm vi báo cáo.
2. Máy chủ kiểm tra quyền và bộ lọc; lấy dữ liệu theo định nghĩa BR-08.
3. Tính theo công thức của chỉ tiêu; hiển thị kỳ, phạm vi, thời điểm đo, số mẫu cùng kết quả.
4. Cho kiểm tra danh sách nguồn trong cùng phạm vi để đối chiếu tổng; không sửa nghiệp vụ từ báo cáo.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Bộ lọc không hợp lệ: from_date sau to_date hoặc đơn vị không tồn tại. → Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác.
- Vượt quyền báo cáo: Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. → Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi.
- Chưa có phản hồi hoặc đổi điểm: Chưa có phản hồi hoặc đổi điểm → Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-RPT-06-01 | Đối chiếu phép tính | Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3. |
| AC-RPT-06-02 | Bộ lọc không hợp lệ | Báo lỗi bộ lọc; không âm thầm chạy một kỳ khác. |
| AC-RPT-06-03 | Vượt quyền báo cáo | Từ chối; không trả số liệu, tên sinh viên hoặc yêu cầu ngoài phạm vi. |
| AC-RPT-06-04 | Chưa có phản hồi hoặc đổi điểm | Không phản hồi: Không có dữ liệu; đổi điểm: số mẫu giữ nguyên, tổng điểm đổi. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-RPT-06-01 | AC-RPT-06-01 | Có điểm 5 và 3; yêu cầu chưa đánh giá không tính. |
| TC-RPT-06-02 | AC-RPT-06-02 | from_date sau to_date hoặc đơn vị không tồn tại. |
| TC-RPT-06-03 | AC-RPT-06-03 | Sinh viên gọi báo cáo hoặc quản lý gửi đơn vị ngoài phạm vi. |
| TC-RPT-06-04 | AC-RPT-06-04 | Chưa có phản hồi hoặc đổi điểm |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Cỡ mẫu 2, trung bình 4, một mức 5 và một mức 3.

Master dùng parent `FR-RPT-06 | Tổng hợp mức hài lòng` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-05
