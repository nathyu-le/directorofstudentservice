# UniSupport Team Charter


## 1 Mục đích

- Thống nhất cách phối hợp để mỗi thành viên biết nhận việc ở đâu, làm theo yêu cầu nào, nộp kết quả như thế nào và hỏi ai khi gặp vướng mắc.
- Quy định cách giao việc, thực hiện, review, kiểm thử và xác nhận hoàn thành các chức năng UniSupport theo PRD đang áp dụng.

## 2 Mục tiêu chung

- Thực hiện đúng phạm vi và tiêu chí chấp nhận trong PRD. Task triển khai chức năng và kết quả kiểm thử phải liên kết với đúng mã chức năng FR; tên chức năng trong Master Plan phải giữ nguyên theo PRD.
- Theo dõi tiến độ bằng Master Plan Excel với lịch Gantt theo ngày. Mỗi task có một người chịu trách nhiệm, đầu ra, hạn dự kiến và các công việc phải hoàn thành trước.
- Chỉ xác nhận chức năng hoàn thành khi đầu ra đã được review, tích hợp và kiểm thử đạt. Việc bàn giao và nghiệm thu được ghi nhận theo kết quả thực tế.

## 3 Vai trò và trách nhiệm

Nhóm tổ chức công việc theo bốn vai trò dưới đây. PM ghi tên người đảm nhiệm trước khi giao việc; người kiêm nhiệm vẫn phải có người khác review đầu ra của mình.

- **PM kiêm PO:** làm rõ yêu cầu, giao việc, sắp xếp phụ thuộc, theo dõi lịch và điều phối vướng mắc. PM quản lý PRD, Master Plan, các quyết định và thay đổi; kiểm tra hồ sơ trước khi xác nhận hoàn thành nội bộ.
- **BE:** thực hiện dữ liệu, API, xử lý nghiệp vụ và quyền truy cập phía máy chủ. BE thống nhất cách kết nối với FE, tự kiểm phần đã làm, cung cấp bản chạy và sửa lỗi backend do QA ghi nhận.
- **FE:** thực hiện giao diện và kết nối API theo PRD. FE phối hợp với BE về dữ liệu và lỗi trả về, tự kiểm luồng thao tác, cung cấp đầu ra để review và sửa lỗi giao diện.
- **QA:** kiểm tra tiêu chí chấp nhận trước khi triển khai, chuẩn bị và chạy ca kiểm thử, ghi kết quả cùng bằng chứng. QA tạo bug, kiểm thử lại sau sửa và báo phần đạt hoặc chưa đạt cho PM.

PM là đầu mối làm việc với bên duyệt yêu cầu và nghiệm thu. Quyết định vượt phạm vi đã thống nhất phải được bên có thẩm quyền xác nhận và lưu lại.

## 4 Quy tắc làm việc

1. **Nhận việc có đủ thông tin:** mỗi task phải có mã, nội dung, người phụ trách, người review, đầu ra, hạn và tiêu chí hoàn thành. Task chức năng ghi mã FR và tiêu chí chấp nhận liên quan. Chưa rõ yêu cầu thì hỏi PM trước khi làm.
2. **Tập trung vào việc đã nhận:** mỗi người thực hiện tối đa hai task triển khai cùng lúc. Muốn đổi người phụ trách hoặc nhận thêm việc phải trao đổi với PM để kiểm tra lịch và công việc phụ thuộc.
3. **Cập nhật trong ngày làm việc:** cuối phiên làm việc, người thực hiện ghi trạng thái, phần đã làm, giờ thực tế, phần còn lại và đường dẫn đầu ra. Chỉ ghi ngày, giờ và kết quả thực tế khi đã phát sinh.
4. **Báo vướng mắc sớm:** báo ngay khi biết task bị chặn hoặc có nguy cơ trễ. Nội dung gồm mã task, nguyên nhân, người cần hỗ trợ và công việc bị ảnh hưởng. PM điều phối trong ngày làm việc kế tiếp; owner cập nhật lại khi được gỡ chặn.
5. **Có người khác review:** mã nguồn và tài liệu thay đổi phải được người khác kiểm tra trước khi merge hoặc phát hành. Người làm xử lý nhận xét, ghi cách kiểm và gửi lại khi cần. BE và FE review phần phối hợp với nhau; QA kiểm hành vi theo PRD.
6. **Thay đổi phải được ghi nhận:** đề nghị đổi yêu cầu, hạn hoặc cách phối hợp được chuyển cho PM. PM kiểm tác động và ghi quyết định trước khi giao phần việc thay đổi. Nhóm cập nhật đồng thời các mục PRD, Master Plan và ca kiểm thử bị ảnh hưởng.
7. **Tôn trọng thời gian và cam kết:** đầu tuần mỗi người báo khả năng tham gia, vắng mặt hoặc hạn chế về thời gian. Người vắng bàn giao task, đầu ra và vướng mắc; PM điều chỉnh người phụ trách và lịch. Nhận xét tập trung vào công việc, kèm căn cứ và hướng xử lý.

## 5 Quy trình nhận và hoàn thành task

1. **Chuẩn bị và giao việc:** PM chọn chức năng từ PRD và chia phần việc phù hợp cho BE, FE, QA. Nhóm kiểm tra yêu cầu, tiêu chí chấp nhận, đầu ra và phụ thuộc. Task chỉ chuyển sang `Ready` khi đủ thông tin và có thể bắt đầu phần việc được giao.
2. **Nhận việc:** người được giao đọc task, xác nhận hiểu yêu cầu và đủ thời gian thực hiện. Nếu thiếu thông tin, ghi câu hỏi và người cần phản hồi. Khi bắt đầu, cập nhật `In Progress` và ngày bắt đầu thực tế trong Master Plan.
3. **Thực hiện và tự kiểm:** BE và FE thống nhất dữ liệu/API trước khi kết nối. Mỗi người làm trên nhánh riêng, tự kiểm phần đã làm và cập nhật task cuối phiên. Nếu dùng dữ liệu giả lập hoặc còn thiếu thành phần, ghi rõ để người nhận biết phần chưa tích hợp.
4. **Nộp và review:** người thực hiện gắn Pull Request hoặc tài liệu vào task, ghi phần thay đổi, cách kiểm và kết quả tự kiểm rồi chuyển `In Review`. Người review ghi nhận xét; người thực hiện sửa và gửi lại. Sau review và tích hợp, nhóm chạy kiểm tra nhanh trên bản thực để xác nhận có thể chuyển `Ready for QA`.
5. **Kiểm thử:** QA nhận đúng bản build, dữ liệu và mã FR, AC, TS liên quan rồi chuyển `In QA`. QA chạy các ca theo PRD, ghi kết quả quan sát và bằng chứng vào `QA_Tracking`. Ca chưa chạy là `Not Run`; đạt là `Pass`; khác kết quả mong đợi là `Fail`; bị chặn là `Blocked` kèm nguyên nhân.
6. **Sửa lỗi và kiểm thử lại:** QA tạo bug có mã FR/AC/TS, bước tái hiện, kết quả mong đợi, kết quả thực tế, bản build và bằng chứng. PM xác định người sửa và thứ tự ưu tiên. BE hoặc FE sửa, tự kiểm và cung cấp bản mới; QA chạy lại ca lỗi cùng các ca liên quan. Bug chỉ đóng khi kiểm thử lại đạt; chưa đạt thì mở lại và ghi phần còn sai.
7. **Xác nhận hoàn thành:** QA tổng hợp kết quả; PM kiểm đầu ra và bằng chứng trước khi xác nhận `Done`. Task từng phần phải đạt tiêu chí của phần đó. Chức năng chỉ hoàn thành khi các phần cần thiết đã tích hợp, tất cả AC đạt, các kiểm tra E2E/NFR liên quan đạt theo giai đoạn, không còn lỗi S1/S2 trong phần nghiệm thu và tài liệu liên quan đã cập nhật. PM ghi ngày kết thúc, tiến độ và giờ thực tế vào Master Plan. Hoàn thành nội bộ được ghi riêng với xác nhận nghiệm thu của bên có thẩm quyền.

Trạng thái task dùng đúng danh sách trong Master Plan: `Backlog`, `Ready`, `In Progress`, `In Review`, `Ready for QA`, `In QA`, `Blocked`, `Done`. Khi bị chặn, owner ghi nguyên nhân và trạng thái cần quay về sau khi xử lý. Task tài liệu hoặc thiết kế hoàn thành sau review; các bước tích hợp và QA áp dụng cho phần triển khai chức năng.

## 6 Gặp vấn đề thì hỏi ai

- **Yêu cầu, phạm vi, ưu tiên, hạn hoặc công việc phụ thuộc:** hỏi PM.
- **API, dữ liệu, quyền hoặc xử lý nghiệp vụ phía máy chủ:** hỏi BE; vấn đề thay đổi hành vi sản phẩm phải báo thêm PM và QA.
- **Giao diện, kết nối API hoặc thao tác người dùng:** hỏi FE; vấn đề dữ liệu/API trao đổi cùng BE.
- **Ca kiểm thử, dữ liệu test, kết quả hoặc mức ảnh hưởng của lỗi:** hỏi QA; PM chốt thứ tự ưu tiên sửa cùng người thực hiện.
- **Người phụ trách vắng hoặc chưa giải quyết được:** báo PM để phân công người hỗ trợ, ghi quyết định cần có và cập nhật lịch. Không mặc định giao việc cho một người dự phòng khi chưa kiểm tra khả năng tiếp nhận.

## 7 Giao tiếp và lưu tài liệu

- **GitHub:** lưu PRD, Team Charter và mã nguồn; dùng issue cho mô tả chi tiết task/bug và Pull Request cho review thay đổi. Task, bug và PR ghi cùng mã liên kết để truy lại yêu cầu và kết quả.
- **Master Plan Excel:** là nơi theo dõi ngày, người phụ trách, phụ thuộc, effort, trạng thái và tiến độ. Sheet `Tasks` giữ kế hoạch cùng số liệu thực tế; `QA_Tracking` giữ kết quả và đường dẫn bằng chứng. Issue trên GitHub dẫn về cùng Task ID và được cập nhật khi trạng thái thay đổi.
- **Kênh chat chung:** nhóm chốt kênh và khung giờ phản hồi khi khởi động. Tin nhắn công việc ghi mã task, vấn đề và việc cần hỗ trợ; người nhận xác nhận tiếp nhận trong một ngày làm việc theo khung giờ đã thống nhất.
- **Họp đầu tuần:** dự kiến thứ Hai, 20 phút, có PM, BE, FE, QA để xác nhận việc tuần, phụ thuộc, khả năng tham gia và người phụ trách.
- **Họp cuối tuần:** dự kiến thứ Sáu, 20 phút, có bốn vai trò để xem kết quả, phần chưa đạt, vướng mắc và việc tiếp theo. PM ghi biên bản ngắn gồm quyết định, người thực hiện và hạn; người vắng đọc và xác nhận phần việc của mình.
- **Quyết định và bằng chứng:** sau trao đổi hoặc họp, người phụ trách cập nhật vào task/tài liệu liên quan trong ngày làm việc. Bằng chứng test ghi bản build và ngày thực hiện, lưu tại đường dẫn dùng chung. PM lưu xác nhận của bên duyệt cùng nội dung, phiên bản và ngày.

Ngày khởi động được nhập trong `Cau_hinh` khi đã chốt. Trước đó, ngày hiển thị trong Master Plan là lịch minh họa. Khi đã có baseline, PM giữ lịch được duyệt để so sánh với dự báo và thực tế; mọi điều chỉnh lịch ghi rõ lý do.

## 8 Ra quyết định và xử lý bất đồng

- **Trong phạm vi kỹ thuật của một phần việc:** BE hoặc FE đề xuất phương án, trao đổi với người bị ảnh hưởng và ghi kết luận vào task. Phương án phải đáp ứng PRD; thay đổi hành vi chuyển PM xử lý.
- **Ảnh hưởng nhiều phần việc:** PM tập hợp ý kiến BE, FE, QA, xem tác động đến yêu cầu, kiểm thử và lịch rồi chốt hướng phối hợp trong thẩm quyền.
- **Đổi yêu cầu hoặc cam kết bàn giao:** PM ghi đề nghị thay đổi, xin xác nhận bên có thẩm quyền, sau đó cập nhật PRD, Master Plan và ca kiểm thử liên quan trước khi triển khai.
- **Có bất đồng:** các bên nêu vấn đề, căn cứ trong PRD hoặc kết quả kiểm, phương án và ảnh hưởng. Nếu chưa thống nhất sau một ngày làm việc, PM điều phối để chốt hướng xử lý hoặc chuyển bên có thẩm quyền; ghi lại việc còn mở và phần bị chặn.
- **PM vắng:** BE, FE và QA tiếp tục các task đủ điều kiện, ghi vấn đề cần quyết định. Trước khi sửa PRD hoặc thay cam kết, phải có xác nhận của PM hoặc người được ủy quyền; việc phụ thuộc vào quyết định đó giữ `Blocked`.

## 9 Thuật ngữ cần biết

- **Task:** một phần việc có mã, người phụ trách, đầu ra và điều kiện hoàn thành.
- **PRD và FR:** PRD là tài liệu yêu cầu; FR là mã một chức năng cụ thể trong tài liệu đó.
- **AC và TS:** AC là tiêu chí chấp nhận; TS là kịch bản để kiểm tra tiêu chí. Kết quả thực tế phải được ghi sau khi chạy.
- **Master Plan và baseline:** Master Plan là kế hoạch theo ngày; baseline là lịch đã được duyệt và lưu để so sánh tiến độ.
- **Pull Request và review:** đề nghị gộp thay đổi trên GitHub và việc người khác kiểm tra trước khi gộp.
- **E2E và NFR:** E2E kiểm luồng qua nhiều chức năng; NFR là yêu cầu phi chức năng trong PRD.
- **Done, S1/S2 và UAT:** Done là hoàn thành theo điều kiện của task; S1/S2 là lỗi nghiêm trọng hoặc lỗi lớn cần xử lý trước khi xác nhận phần nghiệm thu; UAT là kiểm thử nghiệm thu với bên duyệt.

## 10 Xác nhận của thành viên

Mỗi thành viên đọc Charter, xác nhận vai trò và thống nhất thực hiện các quy tắc trước khi nhận việc. Người kiêm nhiệm xác nhận ở các dòng tương ứng. Thông tin dưới đây được điền khi có xác nhận thực tế.

| STT | Họ tên | Vai trò | Ngày xác nhận | Chữ ký hoặc liên kết xác nhận |
| --- | --- | --- | --- | --- |
| 1 |  | PM kiêm PO |  |  |
| 2 |  | BE |  |  |
| 3 |  | FE |  |  |
| 4 |  | QA |  |  |

Sau buổi review cuối tuần, nhóm xem lại những quy tắc cần điều chỉnh. PM ghi lý do, cập nhật phiên bản, thông báo phần thay đổi và lưu xác nhận của các thành viên bị ảnh hưởng trước khi áp dụng.
