# UniSupport — Kế hoạch kiểm soát chất lượng QC

**Phiên bản 3.0 · 04/11/2026 · Kế hoạch thực hiện, chờ review; kết quả chỉ ghi khi có kiểm thử thật.**

## 1. Phạm vi và điều kiện thực thi

QC kiểm tra sản phẩm theo PRD/Master Plan v6.0, ghi expected/actual, bằng chứng, defect và kết quả kiểm lại. [QA.md](QA.md) giữ quy tắc quy trình, quản lý sản phẩm và cổng chất lượng. Phạm vi kiểm chứng gồm bốn module, 24 FR, 149 TC chức năng, tám E2E, sáu NFR và bốn MET.

| Module | FR | TC chức năng |
| --- | --- | --- |
| STU — Cổng hỗ trợ sinh viên | 5 | 32 |
| DSP — Điều phối và xử lý yêu cầu | 10 | 64 |
| RPT — Quản lý và báo cáo | 6 | 37 |
| IAM — Tài khoản và quyền truy cập | 3 | 16 |

Sử dụng bộ case và expected trong repo. Không thêm upload, email/SMS, xuất dữ liệu, tài khoản mới, module hoặc rule tính hạn không có trong baseline. Các rule gắn OQ cần được xác nhận; đạt expected của thiết kế đề xuất không thay quyết định nghiệp vụ.

Repo tham chiếu tại commit `12329e10ebeba603cd84152cc8c662680757aef0` chưa chứa mã ứng dụng/test tự động/CI; 167 kiểm tra nguồn còn `Not Run`. Tài liệu không khẳng định đã có build chạy được. Bản UI chỉ cho phép kết luận phần điều hướng, hiển thị và trạng thái đã kiểm; API, phân quyền server, transaction, dữ liệu lưu và báo cáo thật cần build tích hợp.

### 1.1. Người thực hiện

- **BE/FE:** tự kiểm và unit/component test phù hợp mã thực tế; review chéo; cung cấp bản thiết kế hoặc build, schema/seed và kết quả kiểm tra; sửa lỗi được giao.
- **QA/tài liệu khi làm QC:** chuẩn bị/kiểm fixture; thực thi test độc lập, ghi kết quả, bug, retest/regression và báo cáo.
- **PM/BA:** làm rõ expected, phân định defect/CR, thống nhất ưu tiên và tổ chức review nghiệm thu. Người có thẩm quyền đánh giá/chấp nhận nghiệp vụ; QC không tự ký thay.

### 1.2. Entry criteria và dữ liệu

Chỉ bắt đầu case khi có: FR/AC/TC cùng revision; không còn câu hỏi chặn expected; đúng build/config, schema/seed và actor; quyền truy cập môi trường; fixture đã chuẩn bị và đầu ra predecessor liên quan sẵn. `.QA.01` chuẩn bị dữ liệu; `.BE.03`/`.FE.02` tự kiểm/tích hợp trước `.QA.02` theo task cụ thể.

Mỗi lượt ghi commit/build, OS/browser/version, PHP/MySQL version, viewport, seed revision, actor/scope và thời điểm. Không tự điền version/cấu hình chưa biết. Dữ liệu giả lập cần có:

| Tập dữ liệu | Điều cần chuẩn bị trước |
| --- | --- |
| Người dùng và quyền | Sinh viên A/B, người xử lý A/B, capability dispatch/report, tài khoản inactive; scope và quan hệ hồ sơ xác định |
| Vòng đời và dữ liệu ghi | Yêu cầu ở state thiết kế; question/result/feedback; version mới/cũ, submission_key giống/khác; điều kiện trước và sau cập nhật |
| Báo cáo và thời gian | Mẫu có đáp án tính trước, mẫu rỗng, biên ngày/hạn, nhiều history/feedback để phát hiện join trùng; mốc as_of và timezone rõ |
| NFR và mục tiêu đo | Bộ 1.000 yêu cầu tổng hợp cho NFR-PERF-01; mẫu/tử số/mẫu số hoặc A/B theo MET |

Các nhãn A/B là fixture, không phải người hoặc đơn vị thật. Reset dữ liệu theo từng case, không phụ thuộc dữ liệu sót từ case trước. Expected báo cáo phải có tập nguồn/phép tính độc lập, không lấy số đang hiện trên UI làm đáp án. Server UTC và UI Asia/Ho_Chi_Minh theo thiết kế. Không dùng hồ sơ thật hoặc đưa secret vào evidence.

## 2. Danh mục chức năng và liên kết test

Mỗi FR dưới đây dẫn tới đặc tả gốc; các Test ID nằm trong [149 TC chi tiết](../docs/04-acceptance/test-cases.md). Ma trận [FR–AC–TC](../docs/04-acceptance/traceability-matrix.md) là nguồn mapping. Không sao chép toàn bộ test thành một bản đặc tả thứ hai.

| FR và hành vi | TC theo traceability | Mốc |
| --- | --- | --- |
| [FR-STU-01](../docs/03-modules/01-student-support/requirements/FR-STU-01.md) Gửi yêu cầu hỗ trợ | 8 — TC-STU-01-01…TC-STU-01-08 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) |
| [FR-STU-02](../docs/03-modules/01-student-support/requirements/FR-STU-02.md) Xem danh sách yêu cầu của tôi | 5 — TC-STU-02-01…TC-STU-02-05 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) |
| [FR-STU-03](../docs/03-modules/01-student-support/requirements/FR-STU-03.md) Xem chi tiết và tiến độ yêu cầu | 5 — TC-STU-03-01…TC-STU-03-05 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) |
| [FR-STU-04](../docs/03-modules/01-student-support/requirements/FR-STU-04.md) Bổ sung thông tin được yêu cầu | 7 — TC-STU-04-01…TC-STU-04-07 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-STU-05](../docs/03-modules/01-student-support/requirements/FR-STU-05.md) Phản hồi kết quả hỗ trợ | 7 — TC-STU-05-01…TC-STU-05-07 | [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) |
| [FR-DSP-01](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-01.md) Xem hàng chờ điều phối | 4 — TC-DSP-01-01…TC-DSP-01-04 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) |
| [FR-DSP-02](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-02.md) Xem hồ sơ xử lý và lịch sử nghiệp vụ | 4 — TC-DSP-02-01…TC-DSP-02-04 | [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) |
| [FR-DSP-03](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-03.md) Phân loại yêu cầu | 8 — TC-DSP-03-01…TC-DSP-03-08 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-DSP-04](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-04.md) Phân công trách nhiệm xử lý | 8 — TC-DSP-04-01…TC-DSP-04-08 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-DSP-05](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-05.md) Đặt hạn xử lý dự kiến | 8 — TC-DSP-05-01…TC-DSP-05-08 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-DSP-06](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-06.md) Bắt đầu xử lý yêu cầu | 6 — TC-DSP-06-01…TC-DSP-06-06 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-DSP-07](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-07.md) Yêu cầu sinh viên bổ sung thông tin | 7 — TC-DSP-07-01…TC-DSP-07-07 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-DSP-08](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-08.md) Ghi cập nhật tiến độ xử lý | 6 — TC-DSP-08-01…TC-DSP-08-06 | [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) |
| [FR-DSP-09](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-09.md) Ghi kết quả giải quyết | 7 — TC-DSP-09-01…TC-DSP-09-07 | [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) |
| [FR-DSP-10](../docs/03-modules/02-dispatch-processing/requirements/FR-DSP-10.md) Đóng yêu cầu đã giải quyết | 6 — TC-DSP-10-01…TC-DSP-10-06 | [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) |
| [FR-RPT-01](../docs/03-modules/03-management-reporting/requirements/FR-RPT-01.md) Tổng hợp trạng thái yêu cầu | 6 — TC-RPT-01-01…TC-RPT-01-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FR-RPT-02](../docs/03-modules/03-management-reporting/requirements/FR-RPT-02.md) Theo dõi yêu cầu quá hạn | 6 — TC-RPT-02-01…TC-RPT-02-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FR-RPT-03](../docs/03-modules/03-management-reporting/requirements/FR-RPT-03.md) Thống kê thời gian xử lý | 6 — TC-RPT-03-01…TC-RPT-03-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FR-RPT-04](../docs/03-modules/03-management-reporting/requirements/FR-RPT-04.md) Thống kê vấn đề phổ biến | 6 — TC-RPT-04-01…TC-RPT-04-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FR-RPT-05](../docs/03-modules/03-management-reporting/requirements/FR-RPT-05.md) Thống kê khối lượng theo phòng ban | 6 — TC-RPT-05-01…TC-RPT-05-06 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FR-RPT-06](../docs/03-modules/03-management-reporting/requirements/FR-RPT-06.md) Tổng hợp mức hài lòng | 7 — TC-RPT-06-01…TC-RPT-06-07 | [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) |
| [FR-IAM-01](../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-01.md) Đăng nhập | 6 — TC-IAM-01-01…TC-IAM-01-06 | [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) |
| [FR-IAM-02](../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-02.md) Đăng xuất | 4 — TC-IAM-02-01…TC-IAM-02-04 | [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) |
| [FR-IAM-03](../docs/03-modules/04-accounts-permissions/requirements/FR-IAM-03.md) Kiểm soát quyền theo vai trò và hồ sơ | 6 — TC-IAM-03-01…TC-IAM-03-06 | [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) |

Tổng 24 FR / 149 TC. Mỗi FR giữ một milestone hoàn tất chính; task `.QA.01/.QA.02/.QA.03` lần lượt chuẩn bị fixture, thực thi và báo cáo/theo dõi bug. Lịch từng task lấy từ Master, không tính giờ hoặc số chức năng thêm vì có ba task QC.

## 3. Thực hiện kiểm thử theo tiến trình dự án

### 3.1. Trong quá trình phát triển — MS-01 đến MS-05

Dev tự kiểm và unit/component test phần nghiệp vụ quan trọng theo code có thật: validation, phiên/quyền, chuyển trạng thái/version, retry và phép tính. Chưa có framework/script thì không ghi có automation hoặc coverage tự động. Kết quả tự kiểm/unit phải dẫn được tới revision và cách chạy, không thay QC độc lập.

QC kiểm từng FR theo case: dữ liệu hợp lệ, thiếu/sai/biên, đúng/trái quyền, gửi lặp, concurrency, lỗi giữa giao dịch hoặc biên thời gian nếu FR có rule đó. Khi đủ các chức năng liên quan, kiểm tích hợp UI–API–DB và các luồng:

- STU-01 → DSP-01/DSP-02: cùng mã yêu cầu, đúng chủ, không phát sinh trùng do retry.
- DSP-04 → IAM-03/DSP-02: scope/trách nhiệm đúng sau đổi người phụ trách; người cũ không tiếp tục sửa.
- DSP-07 → STU-03/STU-04 → DSP-09: đúng chủ/câu hỏi, trạng thái và điều kiện giải quyết; giữ dữ liệu/lịch sử theo PRD.
- DSP-09 → STU-05 → DSP-10 → RPT: kết quả, phản hồi, đóng và số liệu đúng quan hệ, quyền và kỳ.

Cuối mốc, chạy đủ case của FR thuộc mốc và regression phần trước có thể bị tác động. Dữ liệu state dựng bằng fixture để kiểm FR không được gọi là toàn luồng E2E đã chạy.

**Các bước cho một case:** đọc Test ID/AC/revision → reset fixture và xác nhận actor/state/version → thực hiện đúng steps → đối chiếu tất cả expected ở response/UI/DB/history theo case → ghi actual/evidence/result → tạo bug hoặc blocker nếu cần. Test trái quyền phải gọi API trực tiếp khi kiểm quyền server; ẩn nút hoặc mock không đủ bằng chứng. Sau fix chạy lại case lỗi rồi regression phần ảnh hưởng, giữ lịch sử kết quả cũ.

### 3.2. Toàn hệ thống trước bàn giao — MS-06

Trên bản tích hợp, đối chiếu đầy đủ 149 TC chức năng và thực hiện 18 kiểm tra hệ thống. Kiểm lại case trên build mới nếu chưa có bằng chứng còn hiệu lực cho bản đó; khi thay đổi có tác động rộng hoặc không chứng minh được phạm vi ảnh hưởng thì chạy lại toàn bộ tập liên quan. Hồ sơ cuối phải truy được kết quả hợp lệ của toàn bộ scope, không ghép Pass từ các bản không tương thích.

| Nhóm | Mã và trọng tâm | Task thực hiện |
| --- | --- | --- |
| E2E tiếp nhận đến hoàn tất | E2E-01 toàn vòng đời; E2E-02 bổ sung thông tin; E2E-03 đổi trách nhiệm; E2E-04 hạn và báo cáo | QA-04 |
| E2E đối chiếu và an toàn | E2E-05 sáu báo cáo khớp nguồn; E2E-06 quyền xuyên luồng; E2E-07 không bỏ sót/gửi trùng | QA-04 |
| Chạy lại để bàn giao | E2E-08: người khác cài theo source/seed/guide và thực hiện E2E-01 trên môi trường sạch | QA-10, cùng NFR-DEP-01 |

Bước và expected đầy đủ nằm trong [system-tests.md](../docs/04-acceptance/system-tests.md). Chạy các test quyền, trạng thái, dữ liệu và số đo trên đúng build/môi trường; kết luận chỉ trong phạm vi prototype được kiểm.

### 3.3. Nghiệm thu nghiệp vụ — UAT

QC chuẩn bị dữ liệu và kịch bản từ PRD/E2E cho sinh viên, nhân viên và quản lý; PM/BA làm rõ expected với người được chỉ định đánh giá. Các luồng gồm gửi/theo dõi, điều phối/bổ sung, trả kết quả/phản hồi/đóng và báo cáo. QC chạy kiểm tra nội bộ trước buổi review, ghi kết quả và lỗi còn lại; người đại diện nghiệp vụ kiểm sự phù hợp, ghi chấp nhận hoặc yêu cầu xử lý.

Kết quả QC kỹ thuật và quyết định UAT/nghiệm thu là hai hồ sơ khác nhau. Chưa có người có thẩm quyền hoặc quyết định thì ghi chờ xác nhận; không tự đặt khách hàng, thời hạn nghiệm thu hay bảo hành từ nhóm mẫu. Handoff prototype không đồng nghĩa đưa hệ thống vào vận hành production.

## 4. Yêu cầu phi chức năng và mục tiêu đo lường

### 4.1. Sáu NFR theo nguồn

| Mã | Nội dung kiểm và expected chính | Task |
| --- | --- | --- |
| NFR-SEC-01 | Kiểm phiên, capability/object scope, CSRF, SQL injection/XSS, mật khẩu băm và dữ liệu/log không lộ secret; hành vi trái quyền bị chặn theo expected. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-REL-01 | Giả lỗi trước commit, gửi lặp và cập nhật version cũ/đồng thời; rollback hoặc một kết quả hợp lệ, không ghi đè/lưu dở dang. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-USAB-01 | Gửi/theo dõi/bổ sung thao tác được ở 1366×768 và 390×844; nhãn/lỗi rõ, bàn phím dùng được, không mất dữ liệu do validation. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-PERF-01 | 1.000 yêu cầu tổng hợp, 20 lần đo theo nguồn; target đề xuất p95 ≤3 giây trên môi trường demo ghi cấu hình. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-OBS-01 | Lỗi có request/correlation ID; history có tác nhân/thời gian/hành động; không lưu mật khẩu/token trong log. | [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) |
| NFR-DEP-01 | Người khác cài môi trường sạch từ source/README/env mẫu/schema/seed và chạy E2E-01; ghi thiếu sót và bằng chứng chạy lại. | [QA-10](https://github.com/nathyu-le/directorofstudentservice/issues/58) |

Viewport và target hiệu năng đang là mục tiêu thử prototype, cần review môi trường theo OQ-07. NFR-PERF-01 dùng 1.000 **yêu cầu mẫu**, không phải 1.000 người dùng đồng thời. Lưu 20 thời gian đo cho từng trang được kiểm, điều kiện máy/dữ liệu và phương pháp p95 đã thống nhất; đối chiếu target nguồn 95% lần đo ≤3 giây. Không công bố kết quả cho môi trường khác hoặc SLA production. Chưa có môi trường/phương pháp thống nhất thì ghi thiếu điều kiện, không kết luận đạt. Kiểm an toàn theo case không phải chứng nhận/pentest toàn diện.

### 4.2. Bốn MET theo PRD

| Mã và mục tiêu | Điều kiện nguồn |
| --- | --- |
| MET-01 — Có người chịu trách nhiệm | M/N>=90%, N>0; M requests có bộ department/assignee hợp lệ. |
| MET-02 — Theo dõi trạng thái | K/N>=90%, N>0; K hồ sơ chủ SV đọc được và khớp DB/lifecycle. |
| MET-03 — Giảm hỏi lại tiến độ | (B-A)/B>=30%, B>0; kịch bản trước/sau tương đương, tập yêu cầu tương đương. |
| MET-04 — Giảm bỏ sót/trùng | Đếm intent/request/hàng chờ trước/sau; không tự đặt ngưỡng% mới. |

Mỗi phép đo ghi dataset/seed, as_of, phạm vi, số đếm và truy vấn/nguồn tính. N=0/B=0 hoặc thiếu mẫu thì không tự tính 0% hay Pass; ghi tình trạng chưa đo/bị chặn đúng thực tế. MET-04 không có ngưỡng phần trăm bổ sung. Kết quả giả lập không suy rộng thành hiệu quả toàn trường. Các mục tiêu này không thay tiêu chí AC và kết quả test.

## 5. Lỗi, retest và sự cố

### 5.1. Phân loại và ưu tiên

Phân nhóm dưới đây là quy ước vận hành đề xuất cho bản kế hoạch, không tuyên bố đã cấu hình nhãn mới trên GitHub.

| Nhóm lỗi | Đánh giá theo ảnh hưởng thực tế | Quyết định tại cổng |
| --- | --- | --- |
| Nghiêm trọng | Chặn luồng chính, kết quả nghiệp vụ sai, sai quyền/lộ hồ sơ hoặc sai/mất dữ liệu; ví dụ gửi tạo hai yêu cầu do cùng key, người cũ vẫn sửa hồ sơ, số báo cáo sai | Ưu tiên khắc phục; FR/cổng liên quan chưa được xác nhận khi còn lỗi. Có workaround không tự làm lỗi dữ liệu/quyền thành thông thường |
| Thông thường | Vấn đề trình bày/hướng dẫn/thao tác có ảnh hưởng giới hạn, không gây các hậu quả trên | Ghi bug, owner và hạn. Chỉ được để lại tại cổng khi không vi phạm AC/TC bắt buộc và có quyết định chấp nhận phù hợp; không biến Fail thành Pass |

Mức độ ảnh hưởng khác với thứ tự ưu tiên. PM/BA cùng QA/BE/FE xác định việc cần sửa trước dựa trên tác động, dependency và mốc; không đặt thời hạn sửa cố định khi chưa thống nhất. Nếu bất đồng về expected, PM/BA xử lý theo PRD/OQ/CR; nếu implementation đúng PRD nhưng cần hành vi khác thì đó là thay đổi cần review.

### 5.2. Ghi nhận và xử lý defect

1. **Ghi lỗi:** tạo GitHub issue loại/nhãn bug phù hợp cấu hình thật, liên kết FR/AC/TC và issue chức năng. Nội dung gồm build, môi trường, seed/actor, tiền điều kiện, steps, expected, actual, tác động, evidence và owner.
2. **Phân công/sửa:** PM/BA thống nhất owner; BE/FE sửa trên nhánh phù hợp, review bản sửa và cung cấp commit/build mới. Issue sửa code hoàn tất chưa đồng nghĩa defect đã xác minh.
3. **Retest và regression:** QC chạy lại steps tái hiện và test bị ảnh hưởng. Session/quyền phải kiểm luồng đọc/ghi và scope; state/assignment phải kiểm vòng xử lý và báo cáo; query phải kiểm mẫu rỗng, biên kỳ và join nhiều bản ghi. Thay UI/API/config thì kiểm luồng phụ thuộc và smoke tương ứng.
4. **Đóng/mở lại:** retest đạt và hoàn tất regression cần thiết mới đóng bug; chưa đạt giữ/mở lại với bằng chứng mới. Duplicate dẫn bug gốc; by design cần căn cứ và quyết định. Không xóa Fail ban đầu.

Lỗi thông thường được chấp nhận để lại cần lý do, tác động, owner/hạn hoặc quyết định không sửa, và phải có trong hồ sơ bàn giao. Nếu vi phạm AC/TC thì FR vẫn chưa đạt trừ khi có CR được duyệt thay baseline. Chi phí/giờ sửa ngoài task hiện có phải được PM đánh giá, không mặc định nằm hết trong `.QA.03`.

### 5.3. Trạng thái kết quả test

| Result | Khi dùng | Bắt buộc ghi |
| --- | --- | --- |
| Not Run | Chưa thực thi trên build/phạm vi báo cáo | Giữ rõ chưa chạy; không tính là Pass |
| Pass | Đủ bước, mọi expected đạt | Actual, build/môi trường/seed/actor, tester, thời điểm và evidence thật |
| Fail | Đã có ít nhất một expected không đạt | Defect ID, actual và tác động; nếu bước sau bị chặn thì ghi thêm blocker, không che Fail bằng Blocked |
| Blocked | Có trở ngại khiến chưa đủ căn cứ kết luận Pass/Fail | Nguyên nhân, bước đã chạy, owner gỡ và dependency |

Tạm dừng nhánh khi build/seed không tái lập, expected mâu thuẫn, môi trường ngừng hoạt động hoặc lỗi chặn khiến kết quả sau không đáng tin. Báo PM/BA, ghi phạm vi và owner. Tiếp tục khi đầu vào/bản sửa đã review, xác định lại build/seed và smoke phù hợp đủ điều kiện; case độc lập vẫn được chạy. Không tự dời ngày Master để che phần chậm.

Sự cố môi trường/quyền/dữ liệu xử lý theo QA mục 6: báo và giới hạn ảnh hưởng, giữ bằng chứng, khôi phục có kiểm soát, sửa nguyên nhân và QC xác minh lại. Khôi phục môi trường không tự làm các case chưa chạy thành Pass hoặc thay thế vòng xử lý defect.

## 6. Lịch và điều kiện mốc

Ngày dưới đây giữ theo Master v6.0; là target, không phải actual.

| Mốc và đầu ra | Ngày dự kiến | Phạm vi kiểm chứng |
| --- | --- | --- |
| [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) Truy cập hệ thống đúng quyền | 06/11/2026 | FR-IAM-01, FR-IAM-02, FR-IAM-03 — 16 TC |
| [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) Tiếp nhận và theo dõi yêu cầu | 16/11/2026 | FR-STU-01, FR-STU-02, FR-STU-03, FR-DSP-01, FR-DSP-02 — 26 TC |
| [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) Điều phối và phối hợp xử lý | 04/12/2026 | FR-DSP-03, FR-DSP-04, FR-DSP-05, FR-DSP-06, FR-DSP-07, FR-DSP-08, FR-STU-04 — 50 TC |
| [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) Trả kết quả và hoàn tất hỗ trợ | 09/12/2026 | FR-DSP-09, FR-STU-05, FR-DSP-10 — 20 TC |
| [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) Theo dõi và báo cáo đúng dữ liệu | 18/12/2026 | FR-RPT-01, FR-RPT-02, FR-RPT-03, FR-RPT-04, FR-RPT-05, FR-RPT-06 — 37 TC |
| [MS-06](https://github.com/nathyu-le/directorofstudentservice/milestone/6) Kiểm chứng và bàn giao chạy lại được | 22/01/2027 | 24 FR / 149 TC + 18 kiểm tra; đủ hồ sơ, chạy lại và quyết định bàn giao |

MS-01…05 phân bổ đủ 149 TC chức năng. MS-06 kiểm chứng toàn bộ scope và tích hợp/bàn giao; không cộng thêm 149 case mới. Đủ ngày hoặc đóng issue báo cáo không thay đủ AC/TC, review, defect/retest và quyết định.

| Task kiểm chứng | Lịch dự kiến | Đầu ra |
| --- | --- | --- |
| [QA-03](https://github.com/nathyu-le/directorofstudentservice/issues/51) | 13/11/2026 | Checklist MS-02, kết quả kiểm luồng tiếp nhận và bug còn mở |
| [QA-04](https://github.com/nathyu-le/directorofstudentservice/issues/52) | 28/12/2026 | Kết quả E2E-01..07, evidence và bug |
| [QA-05](https://github.com/nathyu-le/directorofstudentservice/issues/53) | 29/12/2026 | Kết quả NFR-SEC/REL/USAB/PERF/OBS và bug |
| [QA-06](https://github.com/nathyu-le/directorofstudentservice/issues/54) | 31/12/2026 | Mẫu đo, phép tính, evidence và kết luận |
| [QA-07](https://github.com/nathyu-le/directorofstudentservice/issues/56) | 04/01/2027–05/01/2027 | Báo cáo retest/regression |
| [QA-10](https://github.com/nathyu-le/directorofstudentservice/issues/58) | 07/01/2027 | Kết quả E2E-08 và NFR-DEP-01 trên môi trường sạch |
| [QA-09](https://github.com/nathyu-le/directorofstudentservice/issues/57) | 08/01/2027 | Test report và user/setup guide |

QA-04 và QA-05 cùng phụ thuộc MS-05, có thể thực hiện song song nếu đủ input. QA-06 cần QA-04/BE-06; QA-07 cần BE-07/FE-06/QA-06; QA-10 cần BE-08/FE-07. Sau QA-10 là QA-09 → QA-08 → PM-08 → PM-09 theo Master. Các nhánh sửa/đóng gói giữ dependency trong nguồn, không thay bằng chuỗi tuần tự chung. QA-08 rà hồ sơ ngày 11/01/2027; PM-08 review ngày 12/01; PM-09 trình bày/bàn giao dự kiến 13/01; due MS-06 vẫn là 22/01/2027.

**Exit criteria:** task áp dụng DoD theo loại đầu ra ở QA mục 4.3; parent FR cần đủ AC/TC đạt trên build tích hợp. Cổng/bàn giao cần kết quả đầy đủ của scope tương ứng, không còn lỗi chặn luồng/sai quyền/sai hoặc mất dữ liệu, các lỗi khác có quyết định phù hợp. Bộ bàn giao phải có source/prototype đúng phạm vi, schema/seed/config mẫu không secret, guide và bằng chứng chạy lại E2E-08/NFR-DEP-01. OQ ảnh hưởng nghiệm thu phải có quyết định phù hợp. Thay code/config/seed sau kiểm chứng thì đánh giá và chạy lại phần ảnh hưởng trước dùng hồ sơ cũ để bàn giao.

## 7. Bằng chứng và báo cáo QC

Run record tối thiểu:

```text
Test ID / FR và AC / revision / Build hoặc Commit
Environment / Schema và Seed revision / Actor và Scope
Expected / Actual / Result / ExecutedAt / Tester
Evidence URL / Bug URL / Retest build và kết quả / Regression đã kiểm
Blocker hoặc giới hạn / người review khi có thật
```

Giữ lịch sử từng lần chạy; ảnh phải chỉ ra thao tác/actor/phạm vi, case dữ liệu cần response/log/đối chiếu DB theo expected. Bằng chứng được làm sạch secret/thông tin nhạy cảm. Kết quả lưu/dẫn từ issue và hồ sơ nhóm đã thống nhất, không yêu cầu thêm công cụ hoặc bản test trùng nguồn.

Báo cáo tại `.QA.03`, QA-09 và review mốc gồm phạm vi/build/môi trường; AC và kết quả theo từng nhóm; defect/retest/regression; số đo NFR/MET; phần thiếu/OQ; tác động tới cổng; hành động–owner–hạn và link bằng chứng. QA/PM review báo cáo trước quyết định; chưa có người/ngày thực tế thì không tự điền.

Luôn tách số Pass/Fail/Blocked/Not Run của **149 TC chức năng, 8 E2E, 6 NFR và 4 MET**. Tỷ lệ thực thi = (Pass + Fail) / tổng case thuộc scope; tỷ lệ đạt trong số đã thực thi = Pass / (Pass + Fail). Báo kèm số đếm/mẫu số và thời điểm, mẫu số 0 ghi N/A. Đây không phải code coverage; chưa có dữ liệu test/bug thì không kết luận “0 lỗi”. AC coverage tính riêng theo QA mục 5.

## 8. Nguồn áp dụng

- [PRD và kế hoạch v6.0](../docs/05-project/requirements.json); [scope](../docs/01-overview/scope.md).
- [Quyền](../docs/02-domain/permissions.md), [vòng đời](../docs/02-domain/lifecycle.md), [API](../docs/02-domain/api-contract.md), [từ điển dữ liệu](../docs/02-domain/data-dictionary.md).
- [FR–AC–TC](../docs/04-acceptance/traceability-matrix.md); [test chức năng](../docs/04-acceptance/test-cases.md); [test hệ thống](../docs/04-acceptance/system-tests.md).
- [Master Gantt](../UniSupport_Master_Plan.xlsx); [milestone](../docs/05-project/master-alignment.md); [Team Charter](../TeamCharter.md); [câu hỏi mở](../docs/05-project/open-questions.md).
- [Kế hoạch QA](QA.md).

Khi nguồn thay đổi, cập nhật QC bị ảnh hưởng, mapping và hồ sơ revision theo quy trình CR; không sửa expected hoặc Master chỉ để khớp một kết quả sai.
