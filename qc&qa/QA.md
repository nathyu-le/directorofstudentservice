# UniSupport — Kế hoạch đảm bảo chất lượng QA

**Phiên bản 3.0 · 04/11/2026 · Đề xuất quy trình để nhóm và người có thẩm quyền review.**

## 1. Mục đích và nguyên tắc

QA quy định cách phòng ngừa sai sót, quản lý đầu ra sản phẩm, giám sát việc tuân thủ và cải tiến quy trình. [QC.md](QC.md) quy định kiểm thử sản phẩm, xử lý lỗi và bằng chứng nghiệm thu. Hai tài liệu dùng cùng PRD/Master Plan v6.0: bốn module STU, DSP, RPT, IAM; 24 FR, 149 TC chức năng và 18 kiểm tra hệ thống.

Nguyên tắc áp dụng:

1. Yêu cầu và AC phải rõ trước khi thực hiện phần việc phụ thuộc; chưa rõ thì PM/BA ghi câu hỏi và lấy quyết định.
2. Mọi yêu cầu có test tương ứng; người thực hiện tự kiểm trước khi chuyển cho người khác xác minh.
3. Người viết không tự xác nhận review của mình. Có source thì thay đổi mã phải được review trước khi merge; có build thì kết luận kiểm thử phải gắn đúng build.
4. Chuyển bước dựa trên đầu ra và bằng chứng, không dựa riêng vào ngày đến hạn hoặc trạng thái issue.
5. Ưu tiên xử lý lỗi chặn luồng, sai quyền hoặc làm sai/mất dữ liệu; giữ lịch sử sửa và kiểm tra lại.

**Căn cứ hiện tại:** repo tại commit `12329e10ebeba603cd84152cc8c662680757aef0` có tài liệu/kế hoạch; chưa có mã ứng dụng hoặc cấu hình kiểm thử tự động trong repo được đọc. Các test nguồn còn `Not Run`. Vì vậy đây là quy trình dự kiến, không phải xác nhận đã test, đã review code hay đã nghiệm thu. Thiết kế gắn câu hỏi mở vẫn cần quyết định; không lấy việc test đúng thiết kế đề xuất để coi nghiệp vụ đã được duyệt.

## 2. Trách nhiệm và tuyến quyết định

| Vai trò theo Team Charter | Trách nhiệm chất lượng | Báo cáo và xác nhận |
| --- | --- | --- |
| PM/BA | Quản lý PRD/AC, làm rõ rule; theo dõi phạm vi/đầu ra; kiểm soát thay đổi; tổ chức review mốc | Nhận báo cáo trực tiếp từ BE, FE và QA/tài liệu; ghi quyết định. Phần vượt thẩm quyền hoặc đổi cam kết phải lấy xác nhận của thầy/bên có thẩm quyền |
| BE | Thiết kế dữ liệu/API, phiên/quyền, transaction; unit test hoặc tự kiểm phù hợp implementation; sửa lỗi BE và đóng gói môi trường | Cung cấp PR/commit, kết quả tự kiểm, build/schema/seed và phần còn vướng cho PM/QA |
| FE | Thiết kế trạng thái màn hình, validation, tích hợp API, desktop/mobile; tự kiểm và sửa lỗi FE | Cung cấp bản giao diện/tích hợp, kết quả tự kiểm, PR và lỗi còn mở |
| QA/tài liệu | Review khả năng kiểm thử; kiểm tuân thủ; khi thực hiện QC thì chạy test, ghi evidence, bug/retest và tổng hợp kết quả | Báo cáo chất lượng trực tiếp cho PM/BA; người khác review đầu ra của QA theo Charter |

Mỗi task có một owner; reviewer được phân công trong bốn vai trò này. Chưa có tên/giờ tham gia thật thì chỉ ghi vai trò. QC đưa kết luận kỹ thuật; người có thẩm quyền quyết định nghiệm thu nghiệp vụ, không để tác giả hoặc tester ký thay.

## 3. Quy trình QA và các cổng chất lượng

### 3.1. Hoạt động theo đầu ra

| Khi thực hiện | Cách kiểm soát | Đầu ra và người kiểm |
| --- | --- | --- |
| Trước phát triển | PM/BA cùng BE/FE/QA rà actor, dữ liệu, luồng lỗi, quyền, trạng thái, AC và phụ thuộc; nối AC với TC; chốt phần chặn trước khi giao việc | FR/contract, câu hỏi/decision và traceability được review; gắn `.PM.01`, QA-01/QA-02 |
| Khi thiết kế/triển khai | BE/FE dùng cùng contract; tự kiểm phần thay đổi. QA kiểm fixture/expected và việc nhận đủ đầu vào | File/PR, self-check và fixture `.QA.01`; người review khác tác giả |
| Trước merge/chuyển QC | Reviewer đối chiếu thay đổi với AC, quyền và tính nhất quán dữ liệu; owner sửa phản hồi; bàn giao build/config/seed cho QC | Review record, build và hướng dẫn kiểm tra; `.BE.03`, `.FE.02` và entry criteria trong QC |
| Cuối mốc | QA đối chiếu FR–AC–TC–kết quả, bug/retest, PR review và đầu ra bàn giao; PM kiểm phạm vi và các quyết định còn mở | Biên bản đánh giá mốc: đạt/chưa đạt, phần thiếu, người xử lý và quyết định thật |
| Sau lỗi nghiêm trọng hoặc lặp lại | QA cùng owner tìm nguyên nhân trong yêu cầu/thiết kế/code/test, đề xuất biện pháp phòng ngừa | Hành động có owner, thời hạn và tiêu chí kiểm hiệu lực; xem mục 7 |

Checklist review kỹ thuật tập trung vào các rủi ro của dự án:

- **Phiên và quyền:** actor lấy từ session; capability/scope kiểm ở máy chủ; sinh viên không nhận hồ sơ khác hoặc ghi chú nội bộ; sau đổi người phụ trách phải kiểm quyền hiện hành.
- **Trạng thái và giao dịch:** state/version đúng rule; nghiệp vụ, dữ liệu con và history nhất quán; lỗi trước commit phải rollback; không ghi đè dữ liệu cũ.
- **Retry:** create dùng submission_key; no_change và thao tác chuyển trạng thái xử lý theo FR riêng, không dùng một quy tắc chung cho mọi request.
- **Dữ liệu và giao diện:** validation theo FR, truy vấn tham số hóa, output an toàn, CSRF theo contract; UI xử lý loading/empty/error và lỗi API; ẩn nút không thay quyền server.
- **Báo cáo và bàn giao:** đúng nguồn/công thức/kỳ/scope; fixture có đáp án; cấu hình và evidence không lộ secret; source/seed/guide khớp build.

Các điểm kiểm này lấy từ tài liệu domain và NFR. Khi chưa có implementation, chỉ review được thiết kế; chưa thể xác nhận code tuân thủ hoặc pipeline chạy đạt.

### 3.2. Cổng để chuyển bước

| Cổng | Điều kiện đi tiếp | Người xác minh và bằng chứng |
| --- | --- | --- |
| Sẵn sàng thực hiện — DoR | Task có FR/đầu ra, AC hoặc tiêu chí review, input/predecessor, owner và reviewer; câu hỏi chặn đã được giải quyết cho phần sắp làm | PM/BA cùng BE/FE/QA; link PRD/contract/decision |
| Merge mã nguồn | Thay đổi đúng phạm vi, người khác review và phản hồi chặn đã xử lý; có self-check/unit test liên quan và kết quả thực tế phù hợp mã được giao | Reviewer và owner; PR/commit, kết quả kiểm tra. Không tuyên bố có test tự động khi chưa xây dựng |
| Bàn giao build để test | Có phiên bản, môi trường/config/schema/seed, fixture, quyền truy cập và kết quả tự kiểm/tích hợp liên quan | BE/FE bàn giao, QC kiểm entry criteria; thiếu gì ghi blocker |
| Hoàn thành task/FR | Đáp ứng DoD đúng loại đầu ra tại mục 4.3; FR phải tích hợp và đạt đủ AC/TC, không còn lỗi chặn | Người review/QC theo loại công việc; source/build/test/evidence liên kết |
| Xác nhận milestone/bàn giao | Đủ FR/task phụ thuộc và bằng chứng theo milestone; không còn lỗi chặn luồng, sai quyền hoặc sai/mất dữ liệu; ngoại lệ khác có quyết định phù hợp | QA lập hồ sơ, PM/BA tổ chức review và người có thẩm quyền xác nhận; ngày dự kiến giữ theo Master |

Đầu ra chưa qua cổng được trả về owner cùng điểm chưa đạt. Ngày trên Master là lịch dự kiến; không tự sửa ngày hoặc đóng mốc để làm báo cáo trông đúng kế hoạch.

## 4. Quản lý sản phẩm và kiểm soát thay đổi

Phần này quản lý **yêu cầu, đầu ra và phiên bản của UniSupport**. Không tạo thêm module hoặc một danh mục tính năng tách khỏi PRD.

### 4.1. Nguồn yêu cầu và cách ưu tiên

PM/BA giữ một nguồn yêu cầu chuẩn là PRD và `requirements.json`. Mỗi FR nêu hành vi, actor, AC, TC và milestone hoàn tất; task BE/FE/QA là công việc tạo ra FR, không phải chức năng mới. Issue cha theo dõi FR, task con theo dõi phần việc; không cộng cả hai thành số chức năng hoàn thành.

Khi chọn việc tiếp theo, PM/BA xác định nhu cầu/AC cần đạt, kiểm đầu vào và phụ thuộc với owner, rồi ưu tiên phần đang chặn luồng/nguồn lực hoặc rủi ro quyền, dữ liệu và mốc gần nhất. Những task đủ input được làm song song theo Master; đổi thứ tự làm ảnh hưởng predecessor, effort hoặc ngày phải được đánh giá và cập nhật kế hoạch.

### 4.2. Theo dõi đầu ra và phiên bản

Với mỗi FR, duy trì liên kết: **FR/AC → task → thiết kế/PR/commit → build/seed → test result/bug → milestone/quyết định**. Trên issue ghi phần đã có, phần còn thiếu, owner, blocker và dự báo; không bắt tạo thêm bảng theo dõi trùng dữ liệu.

Phải phân biệt ba kết luận: giao diện đã review, hành vi tích hợp đã kiểm chứng, và đầu ra đã được chấp nhận. Màn hình mock có thể hoàn thành task thiết kế FE nhưng không chứng minh API, phân quyền server, lưu dữ liệu hoặc số liệu thật đã đúng; chưa đủ để đóng parent FR có các AC đó. Tỷ lệ issue Closed không được dùng thay mức hoàn thành sản phẩm. Nếu báo số FR hoàn tất thì chỉ tính FR đã qua DoD, mẫu số là số FR thuộc phạm vi báo cáo.

Trước trình diễn/bàn giao, PM/BA cùng BE/FE/QA xác định một phiên bản gồm source hoặc prototype tương ứng, commit/build, schema/seed, hướng dẫn, bộ test và giới hạn. Hồ sơ ghi rõ bàn giao UI hay bản tích hợp; thiếu đầu ra của milestone thì kết luận chưa đạt, không dùng tên “bản cuối” thay chứng cứ.

### 4.3. Definition of Done theo loại công việc

| Loại đầu ra | Điều kiện hoàn tất |
| --- | --- |
| FR/đặc tả, thiết kế, tài liệu | Đúng phạm vi task; có revision, tiêu chí review và người khác review; câu hỏi chặn phần đầu ra được xử lý hoặc nêu rõ chưa thể hoàn tất |
| Task BE/FE | Đủ source/PR hoặc đầu ra thiết kế theo task; review hoàn tất, có self-check; task tích hợp phải có kết nối thật và build tương ứng |
| Task QC `.QA.02` / báo cáo `.QA.03` | Chạy/ghi kết quả đúng phạm vi task, đủ actual/evidence; Fail có bug, Blocked có owner; báo cáo phản ánh cả lỗi mở. Báo cáo hoàn tất không có nghĩa FR đã đạt |
| Parent FR | Build tích hợp, PR review; toàn bộ AC/TC của FR đạt và không còn lỗi chặn; bug/retest truy được tới build kiểm chứng |
| Milestone và bộ bàn giao | Đáp ứng toàn bộ phạm vi/điều kiện cổng trong Master; có hồ sơ chất lượng, phiên bản bàn giao, vấn đề còn lại và quyết định thật |

DoD của một task chuẩn bị/tài liệu không đòi chạy toàn bộ test của FR. Ngược lại, đóng task code hoặc task báo cáo không thay DoD của parent FR. Đây là điểm kiểm để tránh vừa đếm trùng công việc vừa kết luận sản phẩm đã xong quá sớm.

### 4.4. Lỗi, yêu cầu thay đổi và phản hồi

PM/BA đối chiếu phản hồi với baseline: làm sai yêu cầu hiện có là defect; đề nghị hành vi mới/khác là CR; yêu cầu mơ hồ phải được làm rõ trước khi chốt expected. BE/FE/QA cung cấp tác động kỹ thuật và kiểm thử; không tự sửa expected để bỏ lỗi.

CR ghi nội dung/lý do, FR/AC/TC/task bị ảnh hưởng, phương án, effort, dữ liệu/quyền, dependency và ngày mốc. PM quyết định trong phạm vi được giao; nội dung vượt thẩm quyền hoặc thay cam kết cần xác nhận đúng người trước thực hiện. Quyết định lưu cùng revision; cập nhật đồng bộ PRD, contract, test, manifest, Master và issue/milestone liên quan. QC đánh giá retest/regression; QA kiểm lại tính nhất quán. OQ-02/03/04/06/07 còn ảnh hưởng hành vi và môi trường phải có quyết định phù hợp trước kết luận nghiệm thu.

## 5. Chỉ số chất lượng và báo cáo

Các mục bắt buộc tại cổng lấy từ AC, Charter và điều kiện milestone; bảng này quy định cách kiểm, không ghi số liệu thực thi thay nhóm.

| Chỉ số | Cách đo và nguồn | Mức cần đạt hoặc cách xử lý |
| --- | --- | --- |
| AC đã kiểm chứng | Số AC có đủ các TC liên quan chạy đạt trên build đánh giá / tổng AC trong phạm vi; mỗi AC tính một lần | 100% AC của FR trước hoàn tất parent; đầy đủ phạm vi tại cổng/bàn giao. Liên kết TC không đồng nghĩa đã chạy đạt |
| Lỗi chặn chất lượng đang mở | Số defect chưa khắc phục gây chặn luồng, sai quyền hoặc sai/mất dữ liệu | 0 khi xác nhận cổng; nếu còn thì chưa đạt, dù issue báo cáo đã đóng |
| Review trước merge | PR có review độc lập và xử lý phản hồi trước merge / tổng PR đã merge trong phạm vi kiểm | Mọi thay đổi phải qua review theo Charter; chưa có PR/dữ liệu thì N/A, không tự ghi 100% |
| An toàn quyền và dữ liệu | Kết quả các test quyền/nhất quán, response và dữ liệu thực tế | Không có truy cập trái quyền, rò nội dung hoặc ghi dữ liệu sai theo expected |
| NFR và mục tiêu sản phẩm | Kết quả NFR/MET theo nguồn và điều kiện đo ở QC mục 4 | Đối chiếu đúng target prototype; mẫu/môi trường chưa đủ thì không kết luận đạt |
| Lỗi tái diễn / sau xác minh | Defect mở lại sau retest không đạt hoặc phát hiện ở đầu ra đã xác minh; ghi ID và nguyên nhân, không đếm lặp comment | Dùng để chọn RCA/cải tiến; chưa có dữ liệu thì ghi chưa đo. Không đặt ngưỡng 10% từ nhóm mẫu |

QA/tài liệu tổng hợp kết quả theo task, buổi review tuần khi được nhóm xác nhận và cuối mỗi mốc, báo trực tiếp PM/BA. Owner báo ngay khi có lỗi nghiêm trọng, blocker hoặc nguy cơ ảnh hưởng cổng, không chờ báo cáo tuần. Không tự ấn định giờ họp/kênh hoặc thời hạn phản hồi khác Team Charter.

Một báo cáo chất lượng chỉ cần: **kỳ/phạm vi/build; AC và Pass/Fail/Blocked/Not Run; lỗi chặn và lỗi còn lại; thay đổi/OQ ảnh hưởng; tác động tới mốc; hành động–owner–hạn; quyết định cần lấy; link bằng chứng**. PM/BA phản hồi quyết định vào task và theo dõi tới khi xử lý xong.

## 6. Rủi ro chất lượng và sự cố

| Rủi ro cần kiểm soát | Phòng ngừa và người phụ trách |
| --- | --- |
| Sai quyền hoặc lộ hồ sơ | BE review session/capability/object scope; FE giới hạn hiển thị đúng dữ liệu được cấp; QC kiểm cho phép/từ chối bằng UI và API |
| Sai trạng thái, trách nhiệm hoặc hạn | PM/BA làm rõ rule; BE/FE dùng cùng contract; QC kiểm chuyển trạng thái, đổi người phụ trách và biên thời gian |
| Gửi trùng, cập nhật đè, history dở dang | BE review transaction/version/retry; QC kiểm race, stale version và rollback |
| Số liệu báo cáo sai | BE và QA chuẩn bị tập nguồn/đáp án trước; QC kiểm phạm vi quyền, kỳ, mẫu rỗng và join không nhân bản |
| Thay đổi sát cổng hoặc bản bàn giao không chạy lại | PM/BA kiểm soát CR; BE/FE giữ source/seed/guide cùng build; QC chạy lại phần ảnh hưởng và kiểm môi trường sạch |

**Defect** là sai lệch sản phẩm so với expected; **sự cố** là sự kiện làm gián đoạn môi trường/bản dùng chung hoặc gây ảnh hưởng dữ liệu, quyền truy cập. Sự cố phải báo PM/BA và người phụ trách ngay; BE/FE giới hạn ảnh hưởng, giữ log/bằng chứng và khôi phục bằng phương án đã kiểm tra nếu có. Sau khôi phục, sửa nguyên nhân, review, QC retest/regression rồi PM/BA kết luận hoạt động đã phục hồi. Không coi khôi phục tạm thời là lỗi đã đóng. Phạm vi áp dụng là môi trường prototype/kiểm thử/bàn giao có thật; không tự thêm thời gian bảo hành hoặc cam kết vận hành production.

## 7. Cải tiến và hồ sơ chất lượng

Cuối mốc, hoặc khi lỗi nghiêm trọng/lặp lại xảy ra, QA cùng owner đối chiếu yêu cầu, thiết kế, review và test để tìm nguyên nhân. Phân biệt sửa lỗi cụ thể với hành động ngăn lỗi lặp lại. Biện pháp có thể là làm rõ AC, bổ sung điểm review, sửa fixture hoặc test hồi quy; thay baseline phải qua CR.

Ghi **vấn đề và bằng chứng → nguyên nhân có căn cứ → hành động → owner/hạn → tiêu chí kiểm hiệu lực → kết quả kiểm lại** vào task/biên bản. Reviewer khác owner xác minh trước đóng hành động. Lần review sau kiểm loại lỗi còn lặp lại hay không; không chỉ ghi “đã nhắc nhở”.

Hồ sơ tối thiểu gồm FR/AC/TC và liên kết task; review/decision/CR; kết quả test và bug/retest; biên bản chất lượng mốc; phiên bản và hồ sơ bàn giao. Nguồn đặc tả/kế hoạch nằm ở repo; kết quả/bằng chứng dẫn từ issue hoặc vị trí nhóm xác nhận.

## 8. Nguồn áp dụng

- [PRD và kế hoạch v6.0](../docs/05-project/requirements.json); [scope](../docs/01-overview/scope.md).
- [Quyền](../docs/02-domain/permissions.md), [vòng đời](../docs/02-domain/lifecycle.md), [API](../docs/02-domain/api-contract.md), [từ điển dữ liệu](../docs/02-domain/data-dictionary.md).
- [FR–AC–TC](../docs/04-acceptance/traceability-matrix.md); [test chức năng](../docs/04-acceptance/test-cases.md); [test hệ thống](../docs/04-acceptance/system-tests.md).
- [Master Gantt](../UniSupport_Master_Plan.xlsx); [milestone](../docs/05-project/master-alignment.md); [Team Charter](../TeamCharter.md); [câu hỏi mở](../docs/05-project/open-questions.md).
- [Kế hoạch QC](QC.md).

Khi PRD hoặc kế hoạch được thay đổi có kiểm soát, QA cập nhật tiêu chí và liên kết bị ảnh hưởng cùng revision, rồi kiểm sự nhất quán trước lần review tiếp theo.
