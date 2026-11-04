# KẾ HOẠCH QUẢN TRỊ VÀ GIÁM SÁT DỰ ÁN

## 1. CƠ CẤU QUẢN TRỊ MINH BẠCH VÀ NGUỒN SỰ THẬT DUY NHẤT (SSOT)

Dự án áp dụng mô hình **Mạng lưới Dữ liệu Tập trung**. Toàn bộ dữ liệu của dự án không được tạo ra từ việc "gõ Word/Excel báo cáo" thủ công, mà được trích xuất trực tiếp từ các công cụ quản lý kỹ thuật (Single Source of Truth).

### 1.1. Hệ thống Công cụ Cốt lõi (SSOT)
* **GitHub Boards & Repository:** Quản lý trạng thái Task (To Do, In Progress, Review, Done), tốc độ Commit code.
* **Hệ thống Quản lý Lỗi (Bug Tracker):** Ghi nhận Bug phát sinh từ QA/QC, mức độ nghiêm trọng, thời gian vá lỗi.
* **Bảng theo dõi giờ (Timesheet) Cloud:** Ghi nhận số giờ làm việc thực tế cho từng Task mỗi ngày.
* **Dashboard Tổng hợp:** Tự động tính toán các chỉ số EVM (Tiến độ, Chi phí) và bật cảnh báo Đỏ/Vàng/Xanh.

### 1.2. Ma trận Trách nhiệm Giải trình (Accountability Matrix)
Trong mô hình phẳng, các cá nhân không "báo cáo số liệu cho sếp", mà thực hiện trách nhiệm giải trình dựa trên dữ liệu chung:

| Vai trò | Trách nhiệm với Hệ thống Dữ liệu | Quyền hạn Giám sát & Xử lý |
| :--- | :--- | :--- |
| **Cấp 1: Cấp Thực thi** (Dev, UI/UX, QA) | Cập nhật trung thực trạng thái Task và số giờ làm việc (Actual Hours) lên hệ thống hằng ngày. | Nhìn thấy toàn bộ tiến độ. Có quyền **gắn cờ cảnh báo (Flag/Blocker)** trực tiếp lên hệ thống nếu bị tắc nghẽn, không cần thông qua Lead. |
| **Cấp 2: Phụ trách chuyên môn** (Tech Lead, BA) | Không được phép can thiệp sửa đổi số liệu thô. | Dựa vào Dashboard để phát hiện điểm nghẽn (bottleneck). Đưa ra giải pháp kỹ thuật/nghiệp vụ để hỗ trợ team tháo gỡ Blocker. |
| **Cấp 3: Quản lý Dự án** (PM) | Cập nhật cấu trúc Master Schedule và phân bổ tài nguyên trên phần mềm quản trị. | Phân tích xu hướng (Trend) từ Dashboard. Ra quyết định nới quỹ thời gian dự phòng hoặc điều chuyển nhân sự. Giải trình với cấp trên. |
| **Cấp 4: Cấp Tài trợ & Khách hàng** (Sponsor, Aurora University) | Truy cập Dashboard Dashboard View-only bất cứ lúc nào. | Dựa trên dữ liệu thực tế (thay vì lời hứa của PM) để đưa ra quyết định thay đổi phạm vi, nghiệm thu, giải ngân. |

*(Lưu ý: QA/QC hoạt động độc lập, log bug thẳng lên hệ thống, Tech Lead không có quyền xóa hoặc ẩn bug).*

---

## 2. QUY TRÌNH ĐỒNG BỘ VÀ PHÂN TÍCH DỮ LIỆU (DATA REVIEW CYCLE)

### 2.1. Daily Sync (Đồng bộ Hằng ngày)
* **Thời điểm:** 20h00 hằng ngày trên Discord Voice (Tối đa 15 phút).
* **Nội dung:** PM mở chung một màn hình Kanban Board. Nhóm không báo cáo lại những gì hệ thống đã hiển thị, mà chỉ tập trung trả lời câu hỏi: *"Tôi đang bị chặn (Blocker) ở đâu và cần ai hỗ trợ để tiếp tục?"*.
* **Yêu cầu:** Trước 22h00, toàn bộ thành viên phải hoàn tất việc cập nhật trạng thái thẻ (Ticket) và nhập số giờ (Timesheet).

### 2.2. Weekly Health Check (Khám sức khỏe Tuần)
* **Thời điểm:** Buổi họp tối Chủ Nhật hằng tuần.
* **Nội dung:** Thay vì nghe từng Lead đọc báo cáo, PM xuất bảng thống kê tự động từ hệ thống và cùng nhóm phân tích:
  * *Tiến độ tuần qua (Burndown chart):* Đang đi nhanh hay chậm?
  * *Chất lượng:* Số lượng bug phát sinh mới vs số lượng bug đã đóng?
  * *Rủi ro:* Cập nhật sổ rủi ro dựa trên tình hình thực tế.
* Kết quả phân tích được PM ghi chú thành văn bản tóm tắt gửi cho toàn team.

### 2.3. Báo cáo Tháng (Executive Summary)
* **Thời điểm:** Cuối mỗi tháng.
* **Nội dung:** PM tổng hợp một báo cáo phân tích ngắn gọn gửi Project Sponsor. Trọng tâm là giải trình các chỉ số lệch chuẩn (Đỏ/Vàng) trên hệ thống, kèm theo yêu cầu hỗ trợ nguồn lực (nếu có).

### 2.4. Báo cáo Mốc (Milestone Deliverable)
* **Thời điểm:** Khi kết thúc mỗi mốc trong Master Schedule.
* **Nội dung:** PM trích xuất báo cáo đóng mốc từ hệ thống gửi Aurora University (có sao kê cho Sponsor), bao gồm:
  * Danh sách tính năng (FR) đã hoàn tất.
  * Biên bản xác nhận chất lượng từ QA.
  * Các yêu cầu cần Khách hàng phê duyệt/cung cấp cho mốc kế tiếp.

### 2.5. Real-time Alerts (Cảnh báo Thời gian thực)
* Hệ thống giám sát (CI/CD, Bug Tracker) được thiết lập webhook. Khi xảy ra các sự kiện: Phát hiện Bug Critical (Sập server, Lỗi bảo mật), hoặc một Task vượt ngân sách thời gian quá 150%, hệ thống tự động bắn tin nhắn `@here` lên kênh Khẩn cấp trên Discord.
* Theo Team Charter, người liên quan phải tiếp nhận thông tin và phản hồi trong 1-2 giờ.

---

## 3. CHỈ SỐ GIÁM SÁT VÀ NGƯỠNG CẢNH BÁO TỰ ĐỘNG

Dự án áp dụng hệ thống Cảnh báo Tín hiệu Đèn (Traffic Light) tự động tính toán từ dữ liệu SSOT. **Không một cá nhân nào được phép can thiệp đổi màu cảnh báo.**

| Lĩnh vực | Phương pháp đo tự động | XANH (An toàn) | VÀNG (Cảnh báo sớm) | ĐỎ (Nguy hiểm - Xử lý ngay) |
| :--- | :--- | :--- | :--- | :--- |
| **Tiến độ** | Chỉ số hiệu suất **SPI (EV/PV)** tính từ số lượng Task hoàn thành. | SPI $\ge$ 1.0 (Đúng hoặc nhanh hơn kế hoạch). | 0.85 $\le$ SPI $<$ 1.0 (Có dấu hiệu chậm trễ). | SPI $<$ 0.85 (Trễ nghiêm trọng, nguy cơ vỡ hạn Mốc). |
| **Chi phí / Giờ công** | Chỉ số **CPI (EV/AC)** tính từ Timesheet thực tế. | CPI $\ge$ 1.0 (Sử dụng hiệu quả giờ công). | 0.9 $\le$ CPI $<$ 1.0 (Vượt cơ sở, bắt đầu dùng quỹ dự phòng). | CPI $<$ 0.9 (Dự báo vỡ tổng ngân sách giờ công). |
| **Chất lượng** | Số lượng Bug Critical/High còn tồn đọng (Open). | 0 Bug Critical/High. Các chỉ số test đạt ngưỡng. | Có Bug Critical đang được assign và sửa trong hạn SLA. | Tồn đọng Bug Critical vượt quá 24h hoặc Mốc không đạt test. |
| **Phạm vi** | Số lượng Change Requests (CR) phát sinh. | Không có Yêu cầu thay đổi. | Có CR đang trong quá trình BA đánh giá tác động. | CR được duyệt làm thay đổi kiến trúc gốc hoặc kinh phí cam kết. |

*(Chỉ số SPI và CPI áp dụng công thức Quản lý giá trị đạt được - Earned Value Management).*

**Hành động ứng phó:**
* **Xanh:** Tiếp tục duy trì đà phát triển.
* **Vàng:** Nhóm tự điều chỉnh nội bộ (Swarming - xúm lại xử lý) hoặc PM điều phối lại ưu tiên công việc.
* **Đỏ:** PM bắt buộc phải can thiệp: Sử dụng quỹ thời gian dự phòng, hoặc báo cáo khẩn cấp lên Sponsor/Aurora University nếu sự cố ảnh hưởng đến hợp đồng cam kết.

---

## 4. QUY TRÌNH LEO THANG (ESCALATION) VÀ CHUYỂN CẤP QUYẾT ĐỊNH

Khi phát sinh vấn đề vượt quá khả năng xử lý của cá nhân, dự án áp dụng quy tắc **Escalation (Leo thang)** thay vì bưng bít:

1. **Vấn đề kỹ thuật / Blockers cục bộ:** Lập trình viên thông báo cho Tech Lead. Tech Lead quyết định phương án kỹ thuật nội bộ.
2. **Vấn đề tiến độ / Ngân sách cục bộ:** PM quyết định việc tái phân bổ task hoặc lấy quỹ giờ công dự phòng (Contingency Buffer) ra sử dụng (trong phạm vi tổng ngân sách nhóm).
3. **Vấn đề thiếu hụt tài nguyên nghiêm trọng:** Nếu dự báo vỡ ngân sách, PM phải lập tờ trình báo cáo thẳng lên **Project Sponsor** để xin cấp thêm nhân sự hoặc ngân sách bổ sung.
4. **Vấn đề thay đổi cam kết lõi:** Bất kỳ sự cố hoặc thay đổi nào làm xê dịch ngày bàn giao cuối cùng hoặc phạm vi tính năng cam kết trong PRD đều phải có sự phê duyệt bằng văn bản từ **Aurora University**.

---

## 5. QUY TRÌNH KIỂM SOÁT THAY ĐỔI (CHANGE CONTROL)

Mọi thay đổi (Change Request - CR) về phạm vi, chi phí hoặc tiến độ so với Baseline ban đầu phải trải qua 5 bước nghiêm ngặt:

1. **Ghi nhận (Log):** Khách hàng hoặc thành viên tạo một CR (Ticket) trên hệ thống Jira/GitHub.
2. **Đánh giá (Assess):** BA phối hợp với Tech Lead phân tích tác động của CR đến kiến trúc, giờ công và rủi ro lây nhiễm chéo.
3. **Quyết định (Decide):**
   * Thay đổi nhỏ (Minor): PM có quyền tự duyệt.
   * Thay đổi lớn (Major, ảnh hưởng hợp đồng): PM gửi hồ sơ tác động cho Aurora University duyệt bằng văn bản.
4. **Cập nhật (Update):** Khi CR được duyệt, PM cập nhật Baseline trên Master Schedule. BA cập nhật tài liệu PRD. Mọi thông tin được log vào *Decision Log*.
5. **Giám sát (Monitor):** CR được triển khai dưới dạng một luồng task mới và hiển thị tiến độ trên Dashboard chung.

---

## 6. DANH MỤC TRUNG TÂM DỮ LIỆU VÀ BIỂU MẪU

Toàn bộ dữ liệu quản trị được số hóa và phân quyền truy cập minh bạch trên nền tảng Cloud:

| Hạng mục Dữ liệu | Nền tảng SSOT | Trách nhiệm cập nhật |
| :--- | :--- | :--- |
| **Bảng Theo dõi Giờ (Timesheet)** | Hệ thống quản lý công việc | Hệ thống tự tổng hợp từ input hằng ngày của cá nhân. |
| **Nhật ký Vấn đề (Issue/Bug Log)** | GitHub Issues | Ghi nhận tự động từ quá trình làm việc của QC và Dev. |
| **Sổ Rủi ro (Risk Register)** | Google Drive / Notion | PM & Leads phân tích và cập nhật rủi ro hằng tuần. |
| **Decision Log (Lịch sử Quyết định)** | Kênh `#announcements` Discord & Notion | PM ghi nhận mọi quyết định quan trọng (nhằm truy vết). |
| **Biên bản Khám Sức Khỏe Tuần** | Google Docs / Notion | PM soạn thảo dựa trên phân tích số liệu Dashboard. |
