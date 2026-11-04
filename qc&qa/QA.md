# UniSupport — Kế hoạch đảm bảo chất lượng QA

**Phiên bản:** 2.0 · **Ngày soạn:** 04/11/2026 · **Trạng thái:** Đề xuất áp dụng, chờ nhóm và người có thẩm quyền review.

## 1. Mục đích và căn cứ áp dụng

QA (Quality Assurance) thiết lập, theo dõi và cải tiến cách nhóm tạo ra sản phẩm đúng yêu cầu. Kế hoạch này quy định tiêu chuẩn đầu vào, review, kiểm tra tuân thủ, quản lý thay đổi và hành động phòng ngừa. [QC.md](QC.md) quy định việc thực thi kiểm thử và kiểm soát lỗi trên sản phẩm. Tách tài liệu không có nghĩa tách rời dòng thông tin: QC cung cấp phát hiện và bằng chứng; QA dùng chúng để sửa nguyên nhân trong quy trình.

Phạm vi tham chiếu là UniSupport, web prototype PHP/MySQL với bốn module STU, DSP, RPT và IAM, theo PRD/Master Plan v6.0 tại commit `dfa151a619b914aa77a29fdf05b5675af4e42cf0`. Có 24 FR, 149 TC chức năng và 18 kiểm tra hệ thống. Các con số này là phạm vi thiết kế; không phải số kiểm thử đã đạt.

Ngày 04/11/2026 đã đối chiếu mã, tên, lịch và phụ thuộc trong manifest với 256 dòng của Master_Gantt, 250 issue task, 24 issue cha FR và sáu milestone. Tại lần đọc này, bộ test nguồn vẫn ghi `Not Run`; repo chưa chứa mã ứng dụng, bộ test tự động hoặc cấu hình CI. Điều đó chỉ phản ánh nội dung repo được kiểm tra, không kết luận về tài sản có thể nằm ngoài repo.

**Ranh giới:** các quy tắc kiểm soát mới trong bản QA/QC này là đề xuất quy trình để review; chúng không tự sửa PRD, ngày Master, cơ cấu nhân sự hay điều kiện milestone. Thiết kế còn gắn OQ được dùng làm căn cứ review/kịch bản prototype, không được mô tả là đã được bên nghiệp vụ duyệt. Tài liệu không chứng nhận ISO/CMMI và không xác nhận dự án đã nghiệm thu.

## 2. Mục tiêu chất lượng và cách kiểm soát

| Mục tiêu | Kiểm soát QA áp dụng | Hồ sơ chứng minh khi thực hiện |
| --- | --- | --- |
| Yêu cầu rõ và có thể kiểm thử | Review actor, tiền điều kiện, dữ liệu, luồng, rule, lỗi, AC và phụ thuộc của từng FR; chỉ rõ quyết định còn mở | Phiên bản FR, nhận xét review, quyết định/OQ liên kết |
| Thực hiện thống nhất giữa BE, FE và QC | Review cùng hợp đồng API, quyền và vòng đời; kiểm fixture/expected trước khi chuyển bước | Contract được review, danh sách test/fixture và PR liên quan |
| Ngăn sai quyền, sai trạng thái và dữ liệu không nhất quán | Bắt buộc review các điểm rủi ro tại mục 5; đối chiếu kiểm chứng tương ứng trong QC | Review findings, PR sửa, kết quả QC đúng build |
| Không bỏ sót đầu ra của kế hoạch | Truy vết FR → AC/TC → task → milestone; kiểm cả đầu vào và điều kiện hoàn thành | Traceability và hồ sơ cổng |
| Phát hiện sai lệch quy trình và giảm lặp lỗi | Kiểm tra tuân thủ, phân tích nguyên nhân, hành động khắc phục và phòng ngừa | Finding, RCA, hành động, kết quả kiểm lại |
| Kết luận chất lượng có căn cứ | Chỉ dùng actual/evidence thật, số liệu có mẫu số và phạm vi rõ | Review record, chỉ số có nguồn, báo cáo QC và quyết định |

Không đặt thêm tỷ lệ chất lượng hoặc cam kết tốc độ từ ví dụ trong tài liệu tham khảo. Mục tiêu sản phẩm MET-01…04 và các ngưỡng NFR hiện có được dẫn và kiểm chứng ở QC; các chỉ số quản trị quy trình tại mục 8 có mục đích khác.

## 3. Trách nhiệm và thẩm quyền

| Vai trò hiện có | Trách nhiệm QA | Phối hợp và giới hạn |
| --- | --- | --- |
| PM/BA | Chủ trì làm rõ yêu cầu, quản lý quyết định/OQ/CR, giữ liên kết kế hoạch và tổ chức review cổng | Không tự xác nhận thay thầy hoặc bên có thẩm quyền; thay phạm vi/cam kết phải lấy quyết định đúng người |
| BE | Rà thiết kế dữ liệu, API, phiên, quyền, giao dịch và lỗi; thực hiện review kỹ thuật và tự kiểm phần BE | Cung cấp build, seed và bằng chứng; không dùng self-check thay QC độc lập |
| FE | Rà màn hình, trạng thái, validation, kết nối và khả năng thao tác theo contract | Xác nhận tương thích với BE; không dùng màn hình mock chứng minh backend hoặc phân quyền thật |
| QA/tài liệu | Duy trì checklist, review test plan/case/fixture, theo dõi tuân thủ, tổng hợp chỉ số và RCA | Đây là vai trò có sẵn; trong hoạt động QC có thể đồng thời làm tester nhưng phải phân biệt hai loại hồ sơ |

Mỗi đầu ra có một owner và người review được phân công; tác giả không tự xác nhận review của mình. Review là trách nhiệm trong bốn vai trò hiện hữu, không phải nhân sự thứ năm. Chưa có tên người và xác nhận nhận việc thì chỉ ghi vai trò, không tự gán tài khoản hoặc ngày đã làm.

## 4. Quy trình QA từ yêu cầu đến bàn giao

### 4.1. Phân tích yêu cầu và xác định tiêu chuẩn

**Khi thực hiện:** trước implementation hoặc khi có thay đổi hành vi. **Đầu vào:** FR, tài liệu domain, AC/TC, câu hỏi mở. **Owner:** PM/BA; BE, FE và QA review phần liên quan.

1. Đọc một revision chung; xác định ai được thao tác, điều kiện vào và đầu ra của hành vi.
2. Kiểm trường dữ liệu, rule, trạng thái, quyền theo hồ sơ, luồng lỗi, biên và thao tác gửi lại. Một hành vi phải được hiểu giống nhau ở FE, BE và expected của test.
3. Đối chiếu FR khác để phát hiện mâu thuẫn; ví dụ quyền xem báo cáo không mặc định cho phép đọc nội dung yêu cầu.
4. Kiểm mỗi AC có cách chứng minh và test tương ứng. Với số đo, phải xác định mẫu, thời điểm, công thức và môi trường.
5. Ghi finding/OQ cho chỗ chưa đủ căn cứ. Chỉ chặn phần phụ thuộc quyết định đó; không tự suy diễn để tiếp tục hoặc tự đóng OQ.

**Đầu ra:** FR/contract được review, liên kết AC–TC và danh sách quyết định còn mở. **Điều kiện chuyển bước:** các điểm chặn đối với phần việc sắp làm đã được xử lý; thiết kế đề xuất vẫn được ghi nhãn đúng trạng thái. Gắn với PM-02, QA-01 và task `.PM.01` từng FR trong Master.

### 4.2. Thiết lập và review kế hoạch kiểm thử

QA cùng BE/FE rà kế hoạch QC: phạm vi 24 FR, test level, loại kiểm thử, fixture, môi trường, rủi ro, lịch, entry/exit criteria và báo cáo. Kiểm các trường hợp hợp lệ, dữ liệu sai, trái quyền, ranh giới, gửi lặp/đồng thời và lỗi giao dịch theo từng FR; không bắt mọi FR có cùng số case.

Đối chiếu TC với AC, không tự thêm chức năng từ ví dụ tham khảo. Unit test do Dev chịu trách nhiệm; QA kiểm cách tổ chức và bằng chứng. QC thực thi kiểm thử sản phẩm ở phạm vi được giao. Automation là phương pháp có thể áp dụng ở nhiều level, không phải level thứ năm hoặc bằng chứng đã có pipeline.

**Đầu ra:** kế hoạch QC, case/fixture được review, mapping và điều kiện sẵn sàng. **Task hiện có:** QA-02 và `.QA.01` từng FR. Không nhân đôi estimate vì cùng đầu ra được nhắc trong cả QA và QC.

### 4.3. Kiểm soát thiết kế và coding

BE/FE review contract trước khi tích hợp: dữ liệu vào/ra, lỗi HTTP, quyền, trạng thái, version, transaction, lịch sử và hành vi retry. Dùng bộ kiểm soát kỹ thuật tại mục 5 khi có code. Đầu ra phải chỉ được file/commit, self-check và PR/task tương ứng.

Trong repo hiện tại chưa có implementation để xác nhận coding guideline đã được tuân thủ. Các mục này là tiêu chuẩn thực hiện cho đầu ra tương lai, không phải kết quả audit code đã đạt.

### 4.4. Review thay đổi và chuyển giao QC

1. Owner mở PR hoặc cung cấp thay đổi gắn task/FR; nêu lý do, ảnh hưởng, self-check, cách chạy và dữ liệu thử.
2. Reviewer đọc theo đặc tả, contract và checklist; ghi vấn đề cụ thể tại PR/task, phân biệt lỗi sản phẩm với thay đổi yêu cầu.
3. Owner sửa; reviewer kiểm lại phần bị phản hồi. Chưa xử lý finding chặn thì chưa coi review hoàn tất.
4. Chuyển giao build/commit, cấu hình, schema/seed và test liên quan cho QC; kiểm đủ entry criteria ở QC trước khi chạy.

**Đầu ra:** review record, bản đã review và bộ thông tin bàn giao test. `.BE.03` và `.FE.02` giữ trách nhiệm tự kiểm/tích hợp trong Master. Review code không được dùng thay actual test.

### 4.5. Giám sát tuân thủ trong lúc thực hiện

Thực hiện khi nhận đầu ra, trước review milestone và sau thay đổi có tác động. QA kiểm mapping, review độc lập, build/fixture, actual/evidence, defect/retest và việc đồng bộ kế hoạch. Dùng mục 7; mọi kết luận phải nêu mẫu đã kiểm và revision.

Khi công cụ static analysis hoặc CI được nhóm bổ sung, BE/FE cung cấp cấu hình, phạm vi rule và run log; QA kiểm tính phù hợp và cách xử lý kết quả. Hiện chưa có cấu hình để yêu cầu “pipeline đã xanh”. Không mặc định SonarQube, framework test, license hay một ngưỡng coverage chưa được chọn.

### 4.6. Đánh giá, cải tiến và review bàn giao

Sau một lượt QC có lỗi tái diễn/nghiêm trọng hoặc trước review cổng, QA tổng hợp xu hướng, thực hiện RCA tại mục 9 và đề xuất biện pháp phòng ngừa. PM/BA đánh giá ảnh hưởng kế hoạch. Chỉ kết thúc hành động khi người review kiểm được bằng chứng hiệu lực.

Trước bàn giao, QA rà hồ sơ QC, source/seed/guide, kết quả người khác chạy lại, vấn đề còn mở và quyết định thật. QC đánh giá sẵn sàng kỹ thuật; PM tổ chức review, người có thẩm quyền ra quyết định chấp nhận. QA không tự ký nghiệm thu thay họ.

## 5. Coding guideline và tiêu chuẩn review cho UniSupport

Các yêu cầu nghiệp vụ/kỹ thuật dưới đây truy về tài liệu domain và FR hiện có. Checklist là cách kiểm soát đề xuất; không bổ sung endpoint hoặc chức năng.

| Nhóm kiểm soát | Reviewer cần kiểm gì khi có implementation | Căn cứ |
| --- | --- | --- |
| Cấu trúc và khả năng đọc | Phân tách xử lý phiên/quyền, validation, nghiệp vụ và truy cập dữ liệu theo kiến trúc được review; tên rõ, không lặp rule mâu thuẫn; thay đổi có FR/task | Contract và thiết kế kỹ thuật của task; chưa mặc định framework/quy ước format chưa được chọn |
| Phiên và quyền | Actor lấy từ session; kiểm capability và object scope trên server trước truy vấn/hành động; không tin role/scope/student_id từ client; đọc lại trách nhiệm sau đổi người xử lý | IAM-03 và permissions.md |
| Dữ liệu vào và an toàn đầu ra | Validate theo FR, dùng truy vấn tham số hóa, output encoding, bảo vệ CSRF cho mutation có phiên; không lộ stack trace, SQL, mật khẩu/token | api-contract.md, NFR-SEC-01 |
| Trạng thái và dữ liệu đồng thời | Kiểm state/version, rollback khi lỗi trước commit; nghiệp vụ và history cùng transaction; giữ bất biến khi stale version | lifecycle.md và các FR ghi dữ liệu |
| Retry và gửi lặp | Phân biệt create replay theo submission_key, no_change của DSP-03/04/05, logout lặp an toàn và chuyển trạng thái lặp bị từ chối | FR cụ thể; không áp một quy tắc retry chung cho mọi thao tác |
| FE và tích hợp | Loading/success/empty/error phù hợp thiết kế; xử lý lỗi theo contract; ẩn nút không thay quyền server; không đưa internal history vào màn hình sinh viên | STU/DSP, permissions.md, FE.02 |
| Báo cáo | Đúng scope, tập nguồn, bộ lọc, biên ngày, thời điểm as_of và công thức; tránh join làm tăng số bản ghi | RPT-01…06 và fixture expected |
| Log và cấu hình | Truy vết lỗi/nghiệp vụ theo thiết kế; không secret trong code, repo hoặc evidence; cấu hình mẫu, schema/seed và hướng dẫn chạy tương ứng build | NFR-OBS-01, NFR-DEP-01 |

Review khả năng bảo trì ở mức cần cho prototype: thay một rule phải xác định được nơi sửa, test liên quan và tác động. Không gắn chứng chỉ bảo mật/khả năng truy cập hoặc yêu cầu tải production ngoài baseline.

## 6. Quản lý cấu hình và thay đổi

Giữ cùng revision cho PRD, contract, traceability, test, manifest, Master và mô tả issue. Build dùng để kết luận phải được nhận diện bằng commit; fixture có revision riêng hoặc nằm trong cùng commit. Bằng chứng cũ chỉ chứng minh build cũ.

Khi phát hiện khác biệt:

1. Ghi finding nếu đầu ra không theo yêu cầu đã có; ghi CR nếu đề nghị đổi yêu cầu/cách làm/lịch. Liên kết FR, AC, TC, task và mốc chịu tác động.
2. PM cùng owner đánh giá ảnh hưởng dữ liệu, quyền, test, predecessor/successor, effort và ngày. Phần cần quyết định được ghi rõ người xác nhận.
3. Review và ghi quyết định trước khi thay baseline. Không sửa expected để khớp lỗi implementation.
4. Cập nhật các nguồn bị ảnh hưởng; QC xác định lại retest/regression. Nếu ngày/quan hệ thay đổi thì đồng bộ cả Master, issue body và due milestone.
5. QA kiểm lại sự nhất quán và lưu revision/decision. Không thay số case hoặc FR âm thầm trong báo cáo.

Các quyết định về trạng thái/quyền đóng, danh mục/scope, tham số đầu vào/session, kỳ báo cáo và môi trường NFR còn được theo dõi tại OQ-02/03/04/06/07. Kết quả test theo thiết kế hiện hành không tự đóng các OQ này; trước nghiệm thu phải có quyết định phù hợp.

## 7. Kiểm tra tuân thủ và xử lý sai lệch quy trình

### 7.1. Cách kiểm tra

QA chọn phạm vi theo task/cổng/thay đổi; ghi danh sách đầu ra được xem xét, revision, reviewer và thời điểm thật. Kiểm toàn bộ FR thuộc cổng đối với mapping và kết quả; review kỹ thuật theo phần thay đổi/rủi ro và ghi rõ phạm vi, không gọi đó là audit toàn bộ code nếu chỉ đọc một PR.

| Tiêu chí | Bằng chứng cần xem | Xử lý khi thiếu/sai |
| --- | --- | --- |
| Đầu vào đủ và đúng revision | FR/contract/AC, quyết định liên quan, task có predecessor | Finding; làm rõ/chặn phần chưa đủ input |
| Review độc lập | PR/task có reviewer và nhận xét được xử lý | Trả lại owner, chưa xác nhận review xong |
| Traceability không thiếu hoặc trùng | FR–AC–TC–task–milestone đối chiếu nguồn | Sửa mapping rồi kiểm lại tổng/phạm vi |
| Test có căn cứ | Build, seed, expected/actual, result và evidence | Không chấp nhận checkbox/Pass thiếu chứng cứ |
| Bug có vòng xác minh | Defect, bản fix, retest/regression; lỗi còn mở được báo cáo | Không coi fix hoặc issue đóng là retest đạt |
| Thay đổi đã đồng bộ | CR, tài liệu, Master, issue và test bị ảnh hưởng | Ghi nguồn lệch và owner xử lý |
| Kết luận đúng giới hạn | NFR/MET có mẫu; OQ/ngoại lệ công khai; đúng build | Yêu cầu sửa kết luận, không suy rộng từ mock/giả lập |

### 7.2. Hồ sơ và luồng xử lý finding

Đề xuất trạng thái hồ sơ quy trình: **Ghi nhận → Đang khắc phục → Chờ xác minh → Đóng**; kiểm lại chưa đạt thì trả về Đang khắc phục. Đây là quy ước trong tài liệu, không tuyên bố đã cấu hình GitHub workflow.

Một finding ghi: ID hoặc issue URL; đối tượng/revision; tiêu chí; sự không phù hợp; bằng chứng; tác động/cổng; owner; hành động/hạn được xác nhận; revision sửa; kết quả xác minh; người xác minh và ngày. Nếu sai lệch là lỗi sản phẩm, liên kết defect theo QC thay vì chỉ sửa tài liệu để che lỗi. Chưa có sự kiện thì không điền ngày/kết quả/ chữ ký giả.

## 8. Chỉ số theo dõi chất lượng quy trình

Các định nghĩa sau là đề xuất đo lường cho bản kế hoạch này; chưa có dữ liệu thực thi để tính hoặc ngưỡng KPI mới được phê duyệt. Mỗi báo cáo phải ghi build/phạm vi, kỳ quan sát, tập dữ liệu, người tổng hợp và liên kết nguồn.

| Chỉ số | Định nghĩa để đo nhất quán | Giới hạn |
| --- | --- | --- |
| Bao phủ liên kết yêu cầu | Số FR có đủ AC/TC/task/mốc hợp lệ chia số FR trong phạm vi review | Đo chất lượng truy vết, không phải code coverage hoặc tỷ lệ test Pass |
| Tuân thủ review | Số đầu ra trong mẫu đã đủ review độc lập trước chuyển bước chia tổng đầu ra được kiểm | Công bố mẫu; không suy từ trạng thái Closed |
| Defect density | Số defect sản phẩm duy nhất, đã xác nhận, gán cho build/phạm vi chia KLOC mã ứng dụng của cùng revision | KLOC cần cách đếm thống nhất; loại thư viện ngoài, mã sinh và tài liệu. Chưa có code/size thì N/A; không lấy 24 FR làm KLOC |
| Reopen rate | Số defect duy nhất trong tập đã từng được đánh dấu Resolved nhưng retest buộc mở lại chia số defect duy nhất đã từng Resolved trong cùng tập và kỳ | Một bug mở lại nhiều lần chỉ tính một ở tử số; thiếu lịch sử hoặc mẫu số 0 thì N/A |
| Defect leakage | Với một build/cổng và cửa sổ theo dõi đã chốt: defect duy nhất phát hiện lần đầu sau ranh giới QC chia tổng defect duy nhất phát hiện trước và sau ranh giới đó trong tập đã xác định | Ghi rõ ranh giới (ví dụ QC → UAT), thời gian và cách gán build; bug reopen không phải bug mới. Chưa có giai đoạn sau hoặc mẫu số 0 thì N/A |

Theo dõi xu hướng theo module, loại nguyên nhân và mức độ ảnh hưởng. Không đưa “giảm 60–80% chi phí”, mục tiêu reopen/density/leakage hoặc số liệu tiết kiệm của bài mẫu thành kết quả hay KPI UniSupport. Nếu nhóm đặt ngưỡng sau này, PM phải lưu căn cứ và quyết định rồi cập nhật kế hoạch.

## 9. Phân tích nguyên nhân và cải tiến liên tục

**Kích hoạt RCA:** lỗi gây sai quyền/sai hoặc mất dữ liệu, lỗi lặp lại sau sửa, cùng nhóm lỗi xuất hiện nhiều nơi, hoặc finding quy trình ảnh hưởng cổng. QC giữ bằng chứng lỗi và kết quả retest; QA điều phối phân tích cùng người thực hiện, PM đánh giá tác động.

1. Mô tả sự kiện thật và timeline; xác định FR, build và phạm vi ảnh hưởng. Không mặc định do cá nhân bất cẩn.
2. Đọc bằng chứng, đối chiếu yêu cầu/thiết kế/review/test; dùng 5 Whys khi phù hợp để kiểm tra nguyên nhân có căn cứ.
3. Tách **correction** (sửa lỗi cụ thể) khỏi **corrective/preventive action** (sửa nguyên nhân và ngăn lặp).
4. Giao owner và tiêu chí kiểm hiệu lực: checklist nào đổi, test/fixture nào bổ sung, review nào cần làm lại; tác động vượt task hiện hữu phải được đánh giá.
5. Xác minh bằng revision mới, retest/regression và lần kiểm tra quy trình tiếp theo. Chỉ đóng RCA khi có bằng chứng cho hành động, không chỉ có câu “đã nhắc nhở”.

**Ví dụ phương pháp, không phải lỗi đã xảy ra:** nếu QC phát hiện báo cáo bị nhân bản do join lịch sử, BE sửa query và QC retest là correction. QA kiểm vì sao review/fixture không bắt lỗi; bổ sung checklist cardinality và fixture có nhiều history/feedback, rồi review lại sáu báo cáo là hành động phòng ngừa. Test mới hoặc sửa AC cần đi qua kiểm soát thay đổi; không tự đổi tổng 149 TC.

## 10. Review cổng chất lượng gắn Master Plan

Ngày bên dưới giữ nguyên kế hoạch hiện hành, không phải ngày đã nghiệm thu. Các task đủ input có thể thực hiện song song; cổng phải đủ các dependency và điều kiện đầu ra. QC cung cấp chứng cứ, QA kiểm tính đầy đủ của hồ sơ, PM tổ chức quyết định.

| Mốc và kết quả | Ngày dự kiến | Hồ sơ QA cần kiểm |
| --- | --- | --- |
| [MS-01](https://github.com/nathyu-le/directorofstudentservice/milestone/1) Truy cập hệ thống đúng quyền | 06/11/2026 | Review contract phiên/quyền, PR, seed và hồ sơ QC của IAM; OQ quyền còn mở phải được giải quyết phù hợp. |
| [MS-02](https://github.com/nathyu-le/directorofstudentservice/milestone/2) Tiếp nhận và theo dõi yêu cầu | 16/11/2026 | Review hồ sơ tiếp nhận và theo dõi, mapping cùng mã yêu cầu, quyền public/internal và bằng chứng tích hợp. |
| [MS-03](https://github.com/nathyu-le/directorofstudentservice/milestone/3) Điều phối và phối hợp xử lý | 04/12/2026 | Review nhất quán rule điều phối, người phụ trách, hạn, bổ sung, state/version và tính toàn vẹn lịch sử. |
| [MS-04](https://github.com/nathyu-le/directorofstudentservice/milestone/4) Trả kết quả và hoàn tất hỗ trợ | 09/12/2026 | Review điều kiện kết quả, phản hồi, đóng yêu cầu và hồ sơ regression liên quan. |
| [MS-05](https://github.com/nathyu-le/directorofstudentservice/milestone/5) Theo dõi và báo cáo đúng dữ liệu | 18/12/2026 | Review định nghĩa sáu báo cáo, nguồn tính, fixture expected, phạm vi quyền và phép đo. |
| [MS-06](https://github.com/nathyu-le/directorofstudentservice/milestone/6) Kiểm chứng và bàn giao chạy lại được | 22/01/2027 | Review hồ sơ 149 TC + 18 kiểm tra, defect/retest, nguồn/seed/guide, chạy lại và quyết định bàn giao. |

Lịch review chuẩn bị gắn QA-01 (14/10/2026), QA-02 (15–16/10/2026) và review từng FR. Cuối dự án: QA-10 kiểm chạy lại ngày 07/01/2027; QA-09 tổng hợp ngày 08/01; QA-08 rà hồ sơ ngày 11/01; PM-08 review ngày 12/01; PM-09 trình bày/bàn giao dự kiến ngày 13/01. MS-06 vẫn có due 22/01/2027. Ngày kế hoạch đã đến hoặc đã qua không chứng minh công việc đã làm.

Giữ điều kiện hiện có: không xác nhận cổng khi còn lỗi chặn luồng, sai quyền hoặc làm mất/sai dữ liệu. Ngoại lệ khác phải có tác động và quyết định chấp nhận thật; ngoại lệ không được biến một TC Fail thành Pass. Parent FR chỉ hoàn tất khi đáp ứng điều kiện AC/TC theo baseline. Hướng dẫn không bổ sung task/giờ hoặc tự đóng issue, milestone.

## 11. Đầu ra QA và điều kiện hoàn tất hồ sơ

Tại mỗi lần review phải truy được: đối tượng/revision; owner/reviewer; checklist và kết luận; findings và hành động; CR/OQ liên quan; chỉ số có nguồn nếu đo; báo cáo QC và giới hạn; quyết định chuyển bước. Ghi tại PR/task hoặc nơi nhóm đã thống nhất, dẫn link từ hồ sơ cổng; không bắt tạo một hệ thống quản trị mới.

Trước khi trình review, kiểm:

- QA nói về quy trình/tiêu chuẩn/giám sát/cải tiến; QC giữ chi tiết thực thi sản phẩm.
- Mọi mã, tên, lịch và liên kết truy về nguồn, không lấy ví dụ bài tham khảo làm scope.
- Không có kết quả test, phê duyệt, công cụ đã triển khai hoặc danh tính được điền khi chưa có chứng cứ.
- Các OQ ảnh hưởng đầu ra, findings chặn và rủi ro còn lại được nêu rõ đúng người xử lý.
- Bản tài liệu được nhóm và người có thẩm quyền review trước khi coi là quy trình đã được chấp thuận.

## 12. Nguồn và tài liệu liên quan

- [PRD và task manifest v6.0](../docs/05-project/requirements.json), [phạm vi dự án](../docs/01-overview/scope.md).
- [Vòng đời](../docs/02-domain/lifecycle.md), [quyền](../docs/02-domain/permissions.md), [hợp đồng API](../docs/02-domain/api-contract.md), [từ điển dữ liệu](../docs/02-domain/data-dictionary.md).
- [Ma trận FR–AC–TC](../docs/04-acceptance/traceability-matrix.md), [149 TC chi tiết](../docs/04-acceptance/test-cases.md), [18 kiểm tra hệ thống](../docs/04-acceptance/system-tests.md).
- [Master Gantt](../UniSupport_Master_Plan.xlsx), [điều kiện milestone](../docs/05-project/master-alignment.md), [Team Charter](../TeamCharter.md), [câu hỏi mở](../docs/05-project/open-questions.md).
- [Kế hoạch QC](QC.md).
- ASQ, [Quality Assurance vs Quality Control](https://asq.org/quality-resources/quality-assurance-vs-control): tham chiếu phân biệt vai trò đảm bảo quy trình, kiểm soát đầu ra và audit; không phải chứng nhận cho dự án.
- ISTQB, [CTFL Syllabus v4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf), các mục 2.2, 3 và 5: tham chiếu cách phân loại và thuật ngữ; các kiểm soát cụ thể trong kế hoạch được điều chỉnh theo UniSupport.
- Đoạn tham khảo QA/QC do người dùng gửi ngày 04/11/2026: dùng khung phân tích yêu cầu → tiêu chuẩn → giám sát → RCA/cải tiến; không sao chép quảng cáo hoặc số liệu lợi ích chưa kiểm chứng.
