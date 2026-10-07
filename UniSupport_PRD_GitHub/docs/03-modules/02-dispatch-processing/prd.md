# PRD module: Điều phối và xử lý yêu cầu

Bản tổng hợp để review; file từng FR và requirements.json giữ cùng nội dung.

# [FR-DSP-01] Xem hàng chờ điều phối

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Nhận biết yêu cầu cần phân loại hoặc phân công.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền điều phối trong phạm vi được cấu hình.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| category / status / department | Tùy chọn | Giá trị danh mục và trạng thái hợp lệ. |
| unassigned | Tùy chọn | Lọc chưa có người phụ trách. |
| page | Tùy chọn | Theo CFG-04. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên mở hàng chờ và chọn bộ lọc.
2. Máy chủ giới hạn phạm vi điều phối rồi lọc yêu cầu.
3. Hiển thị mã, loại, trạng thái, đơn vị, người phụ trách và hạn; cho mở chi tiết nghiệp vụ.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Lọc sai: category_id không tồn tại. → Báo tham số sai; không áp dụng loại khác âm thầm.
- Vai trò không được phép: Sinh viên gọi trang/điểm truy cập hàng chờ. → Từ chối; không lộ danh sách toàn trường.
- Sau phân công: Phân công yêu cầu rồi lọc unassigned=true. → Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-01-01 | Yêu cầu mới | Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã. |
| AC-DSP-01-02 | Lọc sai | Báo tham số sai; không áp dụng loại khác âm thầm. |
| AC-DSP-01-03 | Vai trò không được phép | Từ chối; không lộ danh sách toàn trường. |
| AC-DSP-01-04 | Sau phân công | Yêu cầu rời tập chưa phân công nhưng vẫn nằm trong Tất cả nếu thuộc phạm vi. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-01-01 | AC-DSP-01-01 | Tạo yêu cầu Chưa xác định bằng STU-01. |
| TC-DSP-01-02 | AC-DSP-01-02 | category_id không tồn tại. |
| TC-DSP-01-03 | AC-DSP-01-03 | Sinh viên gọi trang/điểm truy cập hàng chờ. |
| TC-DSP-01-04 | AC-DSP-01-04 | Phân công yêu cầu rồi lọc unassigned=true. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Yêu cầu xuất hiện trong hàng chờ chưa phân loại/chưa phân công với đúng mã.

Master dùng parent `FR-DSP-01 | Xem hàng chờ điều phối` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-IAM-03


---

# [FR-DSP-02] Xem hồ sơ xử lý và lịch sử nghiệp vụ

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Đọc thông tin cần xử lý và truy vết thao tác theo trách nhiệm.

## Actor

Điều phối viên / Nhân viên xử lý. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Có quyền trên yêu cầu theo vai trò và phạm vi công việc.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Yêu cầu thuộc phạm vi được xem. |
| history | Chỉ đọc | Người, thời gian, hành động và thay đổi; phân biệt nội bộ/công khai. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Người dùng mở hồ sơ từ hàng chờ hoặc danh sách được giao.
2. Máy chủ kiểm tra vai trò và phạm vi yêu cầu.
3. Hiển thị thông tin sinh viên tối thiểu phục vụ xử lý, nội dung yêu cầu, trách nhiệm và lịch sử có thứ tự.
4. Giao diện chỉ đưa thao tác phù hợp trạng thái và quyền; máy chủ vẫn kiểm tra khi thực hiện.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Mã không có: Mở mã không tồn tại. → Không tìm thấy; không sinh dữ liệu.
- Ngoài phạm vi: Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. → Từ chối, không trả nội dung sinh viên.
- Ghi chú nội bộ: Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. → Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-02-01 | Có lịch sử | Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi. |
| AC-DSP-02-02 | Mã không có | Không tìm thấy; không sinh dữ liệu. |
| AC-DSP-02-03 | Ngoài phạm vi | Từ chối, không trả nội dung sinh viên. |
| AC-DSP-02-04 | Ghi chú nội bộ | Người xử lý có quyền thấy nội bộ; sinh viên không thấy cả qua API trực tiếp. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-02-01 | AC-DSP-02-01 | Yêu cầu đã tiếp nhận, phân loại, phân công. |
| TC-DSP-02-02 | AC-DSP-02-02 | Mở mã không tồn tại. |
| TC-DSP-02-03 | AC-DSP-02-03 | Nhân viên phòng ban B đọc hồ sơ không được giao ở phòng ban A. |
| TC-DSP-02-04 | AC-DSP-02-04 | Thêm tiến độ nội bộ qua DSP-08 rồi SV xem STU-03. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Hiển thị ba loại sự kiện đúng người, thời điểm và giá trị thay đổi.

Master dùng parent `FR-DSP-02 | Xem hồ sơ xử lý và lịch sử nghiệp vụ` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-STU-01, FR-IAM-03


---

# [FR-DSP-03] Phân loại yêu cầu

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Gán loại vấn đề để định tuyến và tổng hợp báo cáo.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu chưa kết thúc, thuộc phạm vi điều phối; danh mục giả lập đang dùng.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| category_id | Bắt buộc | Một loại đang dùng; không nhận ID giả. |
| reason | Khi đổi loại | Lý do không rỗng. |
| record_version | Hệ thống | Kiểm tra phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên mở hồ sơ và chọn loại vấn đề.
2. Máy chủ kiểm tra danh mục, quyền và phiên bản.
3. Lưu loại, lý do nếu đổi, lịch sử loại cũ/mới.
4. Không tự thay người hoặc phòng ban đang phụ trách; phân công qua DSP-04.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Danh mục sai: Gửi loại đã ngừng dùng hoặc ID không có. → Từ chối; loại và lịch sử không thay đổi.
- Không có quyền: Nhân viên thường hoặc sinh viên tự đổi loại. → Từ chối; loại hiện hành giữ nguyên.
- Đã phân công / thao tác đồng thời: Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. → Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-03-01 | Gán loại | Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này. |
| AC-DSP-03-02 | Danh mục sai | Từ chối; loại và lịch sử không thay đổi. |
| AC-DSP-03-03 | Không có quyền | Từ chối; loại hiện hành giữ nguyên. |
| AC-DSP-03-04 | Đã phân công / thao tác đồng thời | Đổi loại hợp lệ giữ trách nhiệm; bản gửi dùng phiên bản cũ bị yêu cầu tải lại. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-03-01 | AC-DSP-03-01 | Yêu cầu Chưa xác định được gán Học vụ. |
| TC-DSP-03-02 | AC-DSP-03-02 | Gửi loại đã ngừng dùng hoặc ID không có. |
| TC-DSP-03-03 | AC-DSP-03-03 | Nhân viên thường hoặc sinh viên tự đổi loại. |
| TC-DSP-03-04 | AC-DSP-03-04 | Đổi loại hồ sơ có người; một lần lưu khác đã tăng phiên bản. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Loại hiện hành là Học vụ; ghi lịch sử; báo cáo nhóm dùng loại này.

Master dùng parent `FR-DSP-03 | Phân loại yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-02


---

# [FR-DSP-04] Phân công trách nhiệm xử lý

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Đặt một đơn vị và một người chịu trách nhiệm hiện tại, kể cả khi phối hợp đổi đơn vị.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu chưa kết thúc; người nhận đang dùng và thuộc đơn vị được chọn.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| department_id | Bắt buộc | Đơn vị mẫu đang dùng. |
| assignee_id | Bắt buộc | Nhân viên hoạt động thuộc đơn vị. |
| reason | Khi đổi | Lý do đổi trách nhiệm không rỗng. |
| record_version | Hệ thống | Khóa kiểm soát cập nhật đồng thời. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên chọn đơn vị và nhân viên phụ trách.
2. Nếu đổi đơn vị/người, nhập lý do phối hợp và xác nhận.
3. Máy chủ kiểm tra quyền, quan hệ người–đơn vị và phiên bản.
4. Thay bộ trách nhiệm trong một giao dịch, giữ trạng thái và ghi người/đơn vị cũ–mới; quyền xử lý chuyển sang người mới.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Người không thuộc đơn vị: Chọn PB-A với nhân viên PB-B. → Từ chối; không tạo trách nhiệm không nhất quán.
- Tự chiếm yêu cầu: Nhân viên không có quyền điều phối gửi assignee_id của mình. → Từ chối; không thay trách nhiệm.
- Đổi trách nhiệm đồng thời: Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. → Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-04-01 | Phân công lần đầu | Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới. |
| AC-DSP-04-02 | Người không thuộc đơn vị | Từ chối; không tạo trách nhiệm không nhất quán. |
| AC-DSP-04-03 | Tự chiếm yêu cầu | Từ chối; không thay trách nhiệm. |
| AC-DSP-04-04 | Đổi trách nhiệm đồng thời | Lịch sử giữ bộ cũ/mới; NV-A mất quyền sửa; cập nhật cũ bị chặn; không có hai người hiện hành. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-04-01 | AC-DSP-04-01 | Giao yêu cầu cho NV-A thuộc PB-A. |
| TC-DSP-04-02 | AC-DSP-04-02 | Chọn PB-A với nhân viên PB-B. |
| TC-DSP-04-03 | AC-DSP-04-03 | Nhân viên không có quyền điều phối gửi assignee_id của mình. |
| TC-DSP-04-04 | AC-DSP-04-04 | Đổi PB-A/NV-A sang PB-B/NV-B với lý do; NV-A sửa từ màn hình cũ. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Có đúng một đơn vị/người hiện hành, giữ trạng thái; SV thấy trách nhiệm mới.

Master dùng parent `FR-DSP-04 | Phân công trách nhiệm xử lý` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-03, FR-IAM-03


---

# [FR-DSP-05] Đặt hạn xử lý dự kiến

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Cung cấp mốc theo dõi quá hạn cho yêu cầu; không tự áp SLA thực tế.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Yêu cầu chưa kết thúc và thuộc phạm vi điều phối.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| due_at | Bắt buộc khi đặt hạn | Ngày giờ không trước created_at; cấu hình múi giờ CFG-06. |
| reason | Khi đổi hạn | Lý do không rỗng. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên chọn hạn dự kiến; nếu đổi hạn thì ghi lý do.
2. Kiểm tra hạn, quyền và phiên bản.
3. Lưu hạn và lịch sử thay đổi, không đổi trạng thái hoặc người phụ trách.
4. Chi tiết và báo cáo quá hạn sử dụng hạn hiện hành.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Hạn trước khi tạo: due_at nhỏ hơn created_at. → Từ chối; hạn cũ giữ nguyên.
- Sai quyền: Sinh viên hoặc nhân viên không điều phối sửa hạn. → Từ chối; không thay hạn.
- Không hạn / đúng ranh giới: Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. → Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-05-01 | Hạn hợp lệ | Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái. |
| AC-DSP-05-02 | Hạn trước khi tạo | Từ chối; hạn cũ giữ nguyên. |
| AC-DSP-05-03 | Sai quyền | Từ chối; không thay hạn. |
| AC-DSP-05-04 | Không hạn / đúng ranh giới | Không hạn không tính quá hạn; đúng bằng hạn chưa quá hạn; chỉ now > due_at và còn mở mới quá hạn. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-05-01 | AC-DSP-05-01 | Hạn hai ngày sau created_at. |
| TC-DSP-05-02 | AC-DSP-05-02 | due_at nhỏ hơn created_at. |
| TC-DSP-05-03 | AC-DSP-05-03 | Sinh viên hoặc nhân viên không điều phối sửa hạn. |
| TC-DSP-05-04 | AC-DSP-05-04 | Hồ sơ không hạn, rồi hồ sơ có due_at bằng thời điểm đo. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Lưu đúng thời điểm; hiện ở tiến độ; giữ trách nhiệm và trạng thái.

Master dùng parent `FR-DSP-05 | Đặt hạn xử lý dự kiến` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-04


---

# [FR-DSP-06] Bắt đầu xử lý yêu cầu

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Xác nhận bắt đầu xử lý yêu cầu đã tiếp nhận.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đã tiếp nhận, có người phụ trách; người thao tác chính là người được giao.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ được giao cho người thao tác. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên mở yêu cầu được giao và chọn Bắt đầu xử lý.
2. Máy chủ kiểm tra trách nhiệm và trạng thái.
3. Chuyển Đã tiếp nhận → Đang xử lý, ghi thời điểm bắt đầu và lịch sử công khai.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Trạng thái sai: Bắt đầu hồ sơ Đã đóng. → Từ chối; không thay trạng thái và thời điểm.
- Người khác thao tác: NV-B bắt đầu hồ sơ giao NV-A. → Từ chối; trách nhiệm giữ nguyên.
- Bấm hai lần: Gửi lặp lần bắt đầu. → Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-06-01 | Bắt đầu đúng | Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ. |
| AC-DSP-06-02 | Trạng thái sai | Từ chối; không thay trạng thái và thời điểm. |
| AC-DSP-06-03 | Người khác thao tác | Từ chối; trách nhiệm giữ nguyên. |
| AC-DSP-06-04 | Bấm hai lần | Không tạo hai lần bắt đầu; started_at ban đầu giữ nguyên. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-06-01 | AC-DSP-06-01 | NV-A bắt đầu yêu cầu Đã tiếp nhận được giao cho mình. |
| TC-DSP-06-02 | AC-DSP-06-02 | Bắt đầu hồ sơ Đã đóng. |
| TC-DSP-06-03 | AC-DSP-06-03 | NV-B bắt đầu hồ sơ giao NV-A. |
| TC-DSP-06-04 | AC-DSP-06-04 | Gửi lặp lần bắt đầu. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Đang xử lý, có started_at và lịch sử; sinh viên thấy tiến độ.

Master dùng parent `FR-DSP-06 | Bắt đầu xử lý yêu cầu` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-04


---

# [FR-DSP-07] Yêu cầu sinh viên bổ sung thông tin

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Nêu thông tin còn thiếu và đặt yêu cầu vào trạng thái chờ trả lời.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đang xử lý; người thao tác được giao; chưa có câu hỏi bổ sung đang mở.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| question | Bắt buộc | Câu hỏi cụ thể không rỗng; CFG-02. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên ghi rõ thông tin còn thiếu và chọn Yêu cầu bổ sung.
2. Máy chủ kiểm tra quyền, trạng thái và không có câu hỏi mở khác.
3. Lưu câu hỏi, chuyển Chờ bổ sung và thêm sự kiện công khai.
4. Sinh viên thấy câu hỏi trong STU-03 và trả lời qua STU-04.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Câu hỏi rỗng: Nội dung chỉ có khoảng trắng. → Không lưu câu hỏi hoặc chuyển trạng thái.
- Ngoài trách nhiệm: NV-B yêu cầu bổ sung hồ sơ giao NV-A. → Từ chối; không lộ thêm dữ liệu.
- Đã có câu hỏi mở: Gửi lần thứ hai khi Chờ bổ sung. → Từ chối/nhắc câu hỏi hiện hành; không có hai câu hỏi mở.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-07-01 | Câu hỏi hợp lệ | Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi. |
| AC-DSP-07-02 | Câu hỏi rỗng | Không lưu câu hỏi hoặc chuyển trạng thái. |
| AC-DSP-07-03 | Ngoài trách nhiệm | Từ chối; không lộ thêm dữ liệu. |
| AC-DSP-07-04 | Đã có câu hỏi mở | Từ chối/nhắc câu hỏi hiện hành; không có hai câu hỏi mở. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-07-01 | AC-DSP-07-01 | Đang xử lý, hỏi Vui lòng cung cấp mã lớp. |
| TC-DSP-07-02 | AC-DSP-07-02 | Nội dung chỉ có khoảng trắng. |
| TC-DSP-07-03 | AC-DSP-07-03 | NV-B yêu cầu bổ sung hồ sơ giao NV-A. |
| TC-DSP-07-04 | AC-DSP-07-04 | Gửi lần thứ hai khi Chờ bổ sung. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Có một câu hỏi mở; Chờ bổ sung; SV thấy đúng câu hỏi.

Master dùng parent `FR-DSP-07 | Yêu cầu sinh viên bổ sung thông tin` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-06


---

# [FR-DSP-08] Ghi cập nhật tiến độ xử lý

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Ghi công việc đã thực hiện để phối hợp và theo dõi.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đang xử lý hoặc Chờ bổ sung; người thao tác được giao.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| content | Bắt buộc | Không rỗng; CFG-02. |
| visibility | Bắt buộc | public hoặc internal; mặc định internal. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên nhập tiến độ và chọn công khai hoặc nội bộ.
2. Máy chủ kiểm tra quyền, trạng thái và nội dung.
3. Thêm sự kiện tiến độ có người ghi, thời gian và mức hiển thị.
4. Giữ trạng thái/trách nhiệm; chỉ nội dung công khai hiện cho sinh viên.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Giá trị sai: Nội dung trống hoặc visibility=secret. → Từ chối; không thêm lịch sử.
- Mất trách nhiệm: NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. → Từ chối; không ghi nội dung mới.
- Tiến độ nội bộ: Ghi ghi chú internal có dấu TEST-INTERNAL. → Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-08-01 | Tiến độ công khai | Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi. |
| AC-DSP-08-02 | Giá trị sai | Từ chối; không thêm lịch sử. |
| AC-DSP-08-03 | Mất trách nhiệm | Từ chối; không ghi nội dung mới. |
| AC-DSP-08-04 | Tiến độ nội bộ | Người có quyền nghiệp vụ đọc được; STU-03/API SV không có dấu TEST-INTERNAL. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-08-01 | AC-DSP-08-01 | Ghi Đang kiểm tra thủ tục, visibility=public. |
| TC-DSP-08-02 | AC-DSP-08-02 | Nội dung trống hoặc visibility=secret. |
| TC-DSP-08-03 | AC-DSP-08-03 | NV-A gửi cập nhật sau khi yêu cầu đã giao NV-B. |
| TC-DSP-08-04 | AC-DSP-08-04 | Ghi ghi chú internal có dấu TEST-INTERNAL. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Lịch sử xử lý và lịch sử SV có nội dung, người và thời điểm phù hợp; trạng thái không đổi.

Master dùng parent `FR-DSP-08 | Ghi cập nhật tiến độ xử lý` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-06


---

# [FR-DSP-09] Ghi kết quả giải quyết

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Ghi kết quả để sinh viên nhận và phản hồi.

## Actor

Nhân viên được giao. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đang xử lý; không còn câu hỏi bổ sung mở; người thao tác được giao.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| resolution | Bắt buộc | Kết quả/hướng dẫn không rỗng; CFG-02. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Nhân viên nhập kết quả và xác nhận Đã giải quyết.
2. Máy chủ kiểm tra quyền, trạng thái và câu hỏi bổ sung.
3. Lưu kết quả công khai, resolved_at, chuyển Đã giải quyết và ghi lịch sử cùng giao dịch.
4. Sinh viên thấy kết quả và có thể phản hồi qua STU-05.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Còn thiếu thông tin / kết quả trống: Chờ bổ sung hoặc resolution rỗng. → Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần.
- Sai người xử lý: NV-B ghi kết quả cho hồ sơ NV-A. → Từ chối; không thay kết quả.
- Gửi lại / dữ liệu cũ: Gửi lại cùng thao tác hoặc dùng record_version cũ. → Không tạo hai kết quả; bản cũ không ghi đè kết quả mới.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-09-01 | Có kết quả | Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung. |
| AC-DSP-09-02 | Còn thiếu thông tin / kết quả trống | Từ chối; không chuyển Đã giải quyết hay ghi kết quả một phần. |
| AC-DSP-09-03 | Sai người xử lý | Từ chối; không thay kết quả. |
| AC-DSP-09-04 | Gửi lại / dữ liệu cũ | Không tạo hai kết quả; bản cũ không ghi đè kết quả mới. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-09-01 | AC-DSP-09-01 | Đang xử lý; ghi Đã hướng dẫn thủ tục xác nhận. |
| TC-DSP-09-02 | AC-DSP-09-02 | Chờ bổ sung hoặc resolution rỗng. |
| TC-DSP-09-03 | AC-DSP-09-03 | NV-B ghi kết quả cho hồ sơ NV-A. |
| TC-DSP-09-04 | AC-DSP-09-04 | Gửi lại cùng thao tác hoặc dùng record_version cũ. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Đã giải quyết, có kết quả và resolved_at; STU-03 hiển thị đúng nội dung.

Master dùng parent `FR-DSP-09 | Ghi kết quả giải quyết` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-06, FR-DSP-07


---

# [FR-DSP-10] Đóng yêu cầu đã giải quyết

**Module:** Điều phối và xử lý yêu cầu

**Nguồn phạm vi:** Proposal §2.1 — phân nhóm, phân công, xử lý, trách nhiệm phòng ban, phối hợp và lịch sử. Hành vi/trường dưới đây là thiết kế prototype suy ra từ năng lực này, chờ review.

## Mô tả

Kết thúc hồ sơ đã có kết quả sau kiểm tra; bảo toàn lịch sử.

## Actor

Điều phối viên. Phân quyền theo IAM-03 và actors-and-roles.md.

## Preconditions

Đã giải quyết và có kết quả; điều phối viên có quyền trên hồ sơ.

## Dữ liệu và giao diện

| Trường | Tính chất | Kiểm tra |
| --- | --- | --- |
| request_id | Bắt buộc | Hồ sơ đã giải quyết. |
| record_version | Hệ thống | Phiên bản hiện hành. |

Giao diện cần thể hiện rõ tên hành động, mã yêu cầu/bộ lọc, kết quả hiện hành, lỗi tại trường và trạng thái đang gửi. Nhãn Việt là bản chính; nhãn Anh được xem xét trong thiết kế (OQ-05). Không coi việc ẩn nút là kiểm soát quyền.

## Main flow

1. Điều phối viên kiểm tra kết quả và chọn Đóng yêu cầu.
2. Máy chủ kiểm tra quyền và trạng thái Đã giải quyết.
3. Chuyển Đã đóng, ghi closed_at và lịch sử; giữ kết quả, trách nhiệm và phản hồi.
4. Ngừng thao tác xử lý; vẫn cho người có quyền xem và sinh viên phản hồi kết quả.

## Business rules

Áp dụng BR-01, BR-05, BR-06 và quy tắc đặc thù trong business-rules.md. Cấu hình CFG được mô tả riêng; các giới hạn chưa phải yêu cầu nguyên văn proposal. Hành động chỉ ghi dữ liệu mà chức năng này sở hữu; không tự tạo hành động khác.

## Alternative / Error flows

- Đóng trước khi giải quyết: Hồ sơ Đang xử lý chưa có kết quả. → Từ chối; không bỏ qua bước giải quyết.
- Sai vai trò: Sinh viên hoặc nhân viên thường gọi thao tác đóng. → Từ chối; trạng thái không đổi.
- Đóng lặp: Gửi lại thao tác đóng; sau đó thử ghi tiến độ. → Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép.
- Lỗi máy chủ/kết nối: báo chưa xác nhận thành công, cho tải lại kiểm tra kết quả; không tuyên bố đã lưu khi chưa có xác nhận. Nếu có ghi, rollback toàn bộ khi lỗi trước commit.

## Acceptance criteria

| Mã AC | Tình huống | Điều kiện nghiệm thu |
| --- | --- | --- |
| AC-DSP-10-01 | Đóng đúng | Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên. |
| AC-DSP-10-02 | Đóng trước khi giải quyết | Từ chối; không bỏ qua bước giải quyết. |
| AC-DSP-10-03 | Sai vai trò | Từ chối; trạng thái không đổi. |
| AC-DSP-10-04 | Đóng lặp | Không tạo hai lần đóng; tiến độ mới bị từ chối; phản hồi kết quả vẫn được phép. |

## Test và edge cases

| Mã TC | Liên kết AC | Dữ liệu/thao tác trọng tâm |
| --- | --- | --- |
| TC-DSP-10-01 | AC-DSP-10-01 | Điều phối viên đóng yêu cầu Đã giải quyết có kết quả. |
| TC-DSP-10-02 | AC-DSP-10-02 | Hồ sơ Đang xử lý chưa có kết quả. |
| TC-DSP-10-03 | AC-DSP-10-03 | Sinh viên hoặc nhân viên thường gọi thao tác đóng. |
| TC-DSP-10-04 | AC-DSP-10-04 | Gửi lại thao tác đóng; sau đó thử ghi tiến độ. |

Case đầy đủ tại test-cases.md; gồm đúng, sai, vượt quyền và ranh giới/trạng thái cũ. Mỗi case cần ghi actual result và evidence; hiện tất cả Not Run.

## Expected result và liên kết Master

Đã đóng; có closed_at; kết quả và lịch sử giữ nguyên.

Master dùng parent `FR-DSP-10 | Đóng yêu cầu đã giải quyết` và các công việc PM, BE, FE, QA. Mã AC/TC được giữ nguyên trong việc QA; estimate baseline PM 1h / BE 2h / FE 2h / QA 1h là dự toán lập lịch, không là kết quả thực tế. QA 1h dành thực thi 4 case nhỏ; soạn case/bộ dữ liệu, kiểm tra xuyên module và retest thuộc công việc dùng chung riêng.

**Phụ thuộc hành vi/luồng:** FR-DSP-09
