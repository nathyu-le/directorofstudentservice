# UniSupport — Milestone và kế hoạch theo đầu ra

Revision 6.1 đối chiếu baseline `937c123a1a`. Dự toán, ngày và mốc của 24 FR giữ theo manifest. Bản 6.1 có 187 TC chức năng và 18 kiểm tra hệ thống, kèm các điểm kiểm sớm NFR.

## Cách đọc kế hoạch

Milestone là điểm xác nhận kết quả nghiệp vụ, 0h/0 ngày. Task tạo đầu ra có effort và dependency. Các task đủ đầu vào được làm song song; không bắt toàn bộ công việc chờ milestone trước. Milestone kế tiếp kế thừa điều kiện milestone trước. FR có một milestone hoàn tất chính; báo cáo QA không tự chứng minh FR đã đạt mọi AC.

Giữ gốc lịch 12/10/2026–22/01/2027 và cửa sổ triển khai backend từng FR trong repo; không tự hứa rút ngắn dự án. Kiểm chứng cuối bắt đầu trong cửa sổ 28/12/2026 của lịch cũ. Ngày task/mốc tính lại theo dependency và phân bổ giả định 4h/role/ngày; FE thiết kế và QA fixture được làm sớm sau contract. Đây không phải availability/actual hay cam kết được duyệt. Chưa khấu trừ ngày nghỉ. Không dùng khoảng trắng Gantt để kết luận nhân sự rảnh.

| Mã | Kết quả cần đạt | Ngày dự kiến | FR hoàn tất tại mốc |
| --- | --- | --- | --- |
| MS-01 | Truy cập hệ thống đúng quyền | 2026-11-06 | FR-IAM-01, FR-IAM-02, FR-IAM-03 |
| MS-02 | Tiếp nhận và theo dõi yêu cầu | 2026-11-16 | FR-STU-01, FR-STU-02, FR-STU-03, FR-DSP-01, FR-DSP-02 |
| MS-03 | Điều phối và phối hợp xử lý | 2026-12-04 | FR-DSP-03, FR-DSP-04, FR-DSP-05, FR-DSP-06, FR-DSP-07, FR-DSP-08, FR-STU-04 |
| MS-04 | Trả kết quả và hoàn tất hỗ trợ | 2026-12-09 | FR-DSP-09, FR-STU-05, FR-DSP-10 |
| MS-05 | Theo dõi và báo cáo đúng dữ liệu | 2026-12-18 | FR-RPT-01, FR-RPT-02, FR-RPT-03, FR-RPT-04, FR-RPT-05, FR-RPT-06 |
| MS-06 | Kiểm chứng và bàn giao chạy lại được | 2027-01-22 | Nghiệm thu xuyên 24 FR |

## MS-01 — Truy cập hệ thống đúng quyền

Đăng nhập/đăng xuất hoạt động; phiên hết hiệu lực bị từ chối; kiểm quyền và phạm vi dữ liệu trên máy chủ, không chỉ ẩn nút. Tài khoản/danh mục dùng seed, không có chức năng tạo tài khoản mới.

Điều kiện đầu vào/công việc bắt buộc: FR-IAM-01.QA.03, FR-IAM-02.QA.03, FR-IAM-03.QA.03, PM-03, PM-04, BE-03, FE-02, QA-02.

Bằng chứng: build/commit, kết quả test với actual/evidence, PR review và biên bản quyết định. Lỗi chặn khiến mốc chưa đạt dù Issue chuẩn bị báo cáo đã đóng. Không ghi Pass, ngày nghiệm thu hoặc người xác nhận trước khi có thật.

## MS-02 — Tiếp nhận và theo dõi yêu cầu

Sinh viên gửi yêu cầu hợp lệ, thấy đúng hồ sơ của mình; điều phối thấy cùng mã trong hàng chờ và hồ sơ xử lý. Kiểm gửi lặp, validation, quyền đọc và lịch sử công khai/nội bộ theo AC. Các trạng thái chưa có thao tác sinh dữ liệu được kiểm bằng fixture; không coi đó là bằng chứng E2E đã chạy.

Điều kiện đầu vào/công việc bắt buộc: MS-01, FR-STU-01.QA.03, FR-STU-02.QA.03, FR-STU-03.QA.03, FR-DSP-01.QA.03, FR-DSP-02.QA.03, PM-06, BE-04, FE-03, QA-03.

Bằng chứng: build/commit, kết quả test với actual/evidence, PR review và biên bản quyết định. Lỗi chặn khiến mốc chưa đạt dù Issue chuẩn bị báo cáo đã đóng. Không ghi Pass, ngày nghiệm thu hoặc người xác nhận trước khi có thật.

## MS-03 — Điều phối và phối hợp xử lý

Phân loại, phân công, đặt hạn, bắt đầu xử lý, cập nhật tiến độ và vòng yêu cầu/bổ sung thông tin hoạt động đúng vai trò, scope, state và version. Kiểm quyền sau đổi người phụ trách, không ghi dữ liệu/lịch sử một phần.

Điều kiện đầu vào/công việc bắt buộc: MS-02, FR-DSP-03.QA.03, FR-DSP-04.QA.03, FR-DSP-05.QA.03, FR-DSP-06.QA.03, FR-DSP-07.QA.03, FR-DSP-08.QA.03, FR-STU-04.QA.03.

Bằng chứng: build/commit, kết quả test với actual/evidence, PR review và biên bản quyết định. Lỗi chặn khiến mốc chưa đạt dù Issue chuẩn bị báo cáo đã đóng. Không ghi Pass, ngày nghiệm thu hoặc người xác nhận trước khi có thật.

## MS-04 — Trả kết quả và hoàn tất hỗ trợ

Ghi kết quả, sinh viên đọc kết quả và phản hồi; đóng yêu cầu theo đúng điều kiện trong PRD. Không thêm email, SMS hay xuất file. Xác nhận đọc tiến độ/lịch sử liên quan không bị hồi quy.

Điều kiện đầu vào/công việc bắt buộc: MS-03, FR-DSP-09.QA.03, FR-STU-05.QA.03, FR-DSP-10.QA.03, PM-07.

Bằng chứng: build/commit, kết quả test với actual/evidence, PR review và biên bản quyết định. Lỗi chặn khiến mốc chưa đạt dù Issue chuẩn bị báo cáo đã đóng. Không ghi Pass, ngày nghiệm thu hoặc người xác nhận trước khi có thật.

## MS-05 — Theo dõi và báo cáo đúng dữ liệu

Sáu báo cáo đúng tập nguồn, phạm vi quyền, bộ lọc, biên thời gian và công thức PRD; đối chiếu với fixture có expected xác định. Bản tích hợp đủ 24 FR; không có chức năng xuất dữ liệu ngoài baseline.

Điều kiện đầu vào/công việc bắt buộc: MS-04, FR-RPT-01.QA.03, FR-RPT-02.QA.03, FR-RPT-03.QA.03, FR-RPT-04.QA.03, FR-RPT-05.QA.03, FR-RPT-06.QA.03, BE-05, FE-04.

Bằng chứng: build/commit, kết quả test với actual/evidence, PR review và biên bản quyết định. Lỗi chặn khiến mốc chưa đạt dù Issue chuẩn bị báo cáo đã đóng. Không ghi Pass, ngày nghiệm thu hoặc người xác nhận trước khi có thật.

## MS-06 — Kiểm chứng và bàn giao chạy lại được

Có kết quả thật của 187 TC chức năng và 18 kiểm tra hệ thống; đối chiếu toàn bộ AC. Không còn lỗi chặn luồng, sai quyền hoặc làm mất/sai dữ liệu; lỗi khác phải có ghi nhận tác động và quyết định chấp nhận. Có build/commit, test report, source/seed/guide và bằng chứng người khác chạy lại. Kết quả MET dưới mục tiêu phải được báo cáo, không tự coi đạt; xác nhận nghiệm thu bởi người có thẩm quyền, không giả chữ ký.

Điều kiện đầu vào/công việc bắt buộc: MS-05, PM-09, PM-05, QA-08, QA-09, QA-10.

Bằng chứng: build/commit, kết quả test với actual/evidence, PR review và biên bản quyết định. Lỗi chặn khiến mốc chưa đạt dù Issue chuẩn bị báo cáo đã đóng. Không ghi Pass, ngày nghiệm thu hoặc người xác nhận trước khi có thật.

## Ngân sách và giới hạn nguồn lực

288h chính: PM 53h; BE 97.5h; FE 64h; QA 73.5h. 32h dự phòng chưa phân task, tổng 320h. Giữ nguyên effort từng chức năng; việc dùng chung được gắn lại đúng đầu ra. PM-05 là 1h/tuần trong 15 tuần, không làm 15h liên tục hoặc full-time cả thanh Gantt. Giờ sửa lỗi giữ nguyên dự toán, không đảm bảo bao phủ mọi lỗi chưa biết; vượt phải xét dự phòng/thay đổi.

## GitHub, Excel và dữ liệu chuẩn

[Manifest](requirements.json) là nguồn task, FR, ngày, giờ, predecessor và FR → milestone (trường `traceability[].milestone`). [Workbook](../../UniSupport_Master_Plan.xlsx) gồm Master_Gantt, Milestones, WBS, Resources và Change log. Mỗi task công việc có một issue và mốc đích. Issue cha FR không cộng effort. GitHub milestone due là ngày cổng, ngày task giữ trong body.

Mỗi Issue ghi input/output/DoD, link PRD, AC/TC, role, estimate, start/due và links predecessors. Assignee chưa gán khi chưa biết người thật. Link trong body chỉ là truy vết; không tuyên bố đã tạo native dependency. Tiến độ đóng Issue không thay kiểm tra điều kiện nghiệm thu. TC đi theo các task QA hiện có. Rà effort mới qua [R-08](risk-register.md) và [OQ-08](open-questions.md).

Áp dụng [quy trình CR](change-control.md) khi đổi phạm vi, ngày, giờ hoặc phụ thuộc. Gantt phản ứng theo Start/Due, kế hoạch phân bổ và successor cần được rà khi lập lại lịch.

## Kiểm thử và thứ tự bàn giao

Điểm kiểm sớm theo [NFR của PRD](../01-overview/non-functional-requirements.md) trên các task QA trước MS-01 đến MS-05. QA ghi bằng chứng trên issue liên quan. FE tự kiểm giao diện trên các task tích hợp hiện có.

Fixture và FE.01 được chuẩn bị sau contract, song song với BE khi đủ đầu vào. FE.02 chỉ tích hợp khi backend tương ứng đã self-check. QA.02 chỉ chạy sau fixture và FE.02. Kiểm quyền/validation/concurrency được kiểm ở từng FR từ sớm, không để tới cuối.

QA-04 chạy E2E-01..07; QA-05 chạy 5 NFR trừ DEP; QA-06 đo 4 MET. BE/FE sửa trước QA-07 retest/regression. BE-08 và FE-07 chuẩn bị gói/hướng dẫn, sau đó QA-10 chạy E2E-08 và NFR-DEP-01 trên môi trường sạch. QA-09 chốt test report sau QA-10; QA-08 rà hồ sơ; PM-08 review rồi PM-09 bàn giao. Không kiểm chạy lại trước khi có gói/hướng dẫn. Thay code sau kiểm chứng phải đánh giá và chạy lại test bị ảnh hưởng.

## Điều kiện cho mượn nhân sự

Cần lịch tham gia thật, effort còn lại, task/nguồn lực đang chặn, hạn quay lại và tính lại ảnh hưởng mọi successor/milestone. 4h/ngày chỉ là giả định lập lịch, không là số giờ rảnh đã xác nhận. Chưa có các dữ liệu đó thì không kết luận cho mượn được.

## Lịch sử thay đổi

Bản 6.1 ngày 25/11/2026 thêm bốn sheet và điểm kiểm sớm. Theo Resources, tải BE cao nhất tuần 4 (18 giờ) và tải QA cao nhất tuần 2 (10,5 giờ). Nếu BE giảm còn 2 giờ/ngày, tuần 4 là tuần đầu vượt tải (8 giờ). Cần lập lại thứ tự và dự báo MS-01 vì đây là tuần kết thúc cổng MS-01. Người xác nhận ghi tại Change log khi có quyết định.

Thay MS-01..06 cũ theo tuần bằng sáu cổng kết quả trên; giữ mã milestone để không mất liên kết. Giữ nguyên mã task để truy vết nhưng đổi tên công việc dùng chung còn gắn W6/W7/W11/W14/W15. Không thêm tạo tài khoản, email/SMS hoặc xuất dữ liệu từ ảnh minh họa. Phạm vi chức năng và form đặc tả giữ nguyên.
