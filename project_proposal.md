# UniSupport - Hệ thống quản lý hỗ trợ sinh viên

**Khách hàng:** Aurora University  
**Nhóm:** Solo Leveling  

## 1. MỤC TIÊU CỤ THỂ
Lập kế hoạch và thiết kế giải pháp UniSupport giúp Aurora University quản lý tập trung việc tiếp nhận, phân công, xử lý và theo dõi yêu cầu hỗ trợ sinh viên. Kết quả của giai đoạn này được thể hiện qua prototype và bộ tài liệu dự án.

| Mục tiêu | Kết quả cần hướng tới |
|---|---|
| **Trách nhiệm xử lý** | Ít nhất 90% yêu cầu có người chịu trách nhiệm rõ ràng. |
| **Theo dõi yêu cầu** | Ít nhất 90% yêu cầu có trạng thái có thể theo dõi. |
| **Tra cứu tiến độ** | Giảm ít nhất 30% số lần sinh viên phải liên hệ để hỏi tiến độ. |
| **Tính đầy đủ của hồ sơ** | Giảm số yêu cầu bị bỏ sót hoặc xử lý trùng lặp. |
| **Thông tin quản lý** | Nhận biết yêu cầu quá hạn, vấn đề sinh viên thường gặp và khối lượng xử lý của phòng ban. |
| **Quyền truy cập** | Dữ liệu chỉ được truy cập bởi người có thẩm quyền. |

## 2. PHẠM VI THỰC HIỆN

### 2.1. In scope
Phạm vi UniSupport được tổ chức thành bốn module. Các module bao quát nghiệp vụ bắt buộc của đề bài và phục vụ ba nhóm sử dụng chính: sinh viên, nhân viên xử lý và ban quản lý.

| Module | Nội dung tổng thể |
|---|---|
| **Cổng hỗ trợ sinh viên** | Tập trung việc gửi yêu cầu, cung cấp thông tin, theo dõi tiến độ và phản hồi kết quả. Sinh viên biết yêu cầu đã được tiếp nhận và khi nào cần bổ sung thông tin. |
| **Điều phối và xử lý yêu cầu** | Gộp phân loại, phân công và xử lý trong cùng một module. Phòng ban xác định người chịu trách nhiệm, phối hợp giải quyết và lưu lại diễn biến xử lý. |
| **Quản lý và báo cáo** | Cung cấp cái nhìn tổng hợp về tình trạng yêu cầu, thời gian xử lý, vấn đề phổ biến, khối lượng công việc và mức độ hài lòng của sinh viên. |
| **Tài khoản và quyền truy cập** | Quản lý xác thực và quyền sử dụng theo vai trò, giới hạn truy cập thông tin sinh viên và tài liệu liên quan. |

### 2.2. Out of scope
Trong giai đoạn hiện tại, phạm vi không bao gồm các nội dung sau, theo giới hạn của đề bài:

| Nội dung ngoài phạm vi | Giới hạn của giai đoạn hiện tại |
|---|---|
| **Sản phẩm chính thức trên toàn trường** | Không xây dựng sản phẩm production hoàn chỉnh hoặc triển khai sử dụng trên quy mô toàn trường. |
| **Tích hợp và dữ liệu thật** | Không tích hợp thực tế với toàn bộ hệ thống của trường. Prototype và kiểm thử sử dụng dữ liệu giả lập, không sử dụng dữ liệu cá nhân thật. |
| **Nghiệp vụ ngoài hỗ trợ sinh viên** | Không thay thế hệ thống quản lý đào tạo, xử lý thanh toán học phí hoặc tự động đưa ra quyết định học vụ. |

Các nội dung này không làm thay đổi phạm vi hỗ trợ sinh viên đã xác định trong bốn module. Việc mở rộng sang giai đoạn triển khai thực tế cần được xem xét riêng.

### 2.3. Định hướng giải pháp
UniSupport được xây dựng dưới dạng website sử dụng PHP và MySQL. Prototype thể hiện các module trên một hệ thống thống nhất để người dùng thử và nhóm dự án đánh giá mức độ đáp ứng yêu cầu.
Việc thiết kế và kiểm thử tập trung vào khả năng sử dụng, tính nhất quán của thông tin yêu cầu và quyền truy cập theo vai trò. Chi tiết thiết kế được trình bày trong tài liệu dự án.

## 3. KẾ HOẠCH TRIỂN KHAI
Thời gian dự án là 15 tuần. Kế hoạch dưới đây là phương án phân kỳ triển khai, giữ hai mốc của đề bài: đánh giá giữa kỳ vào tuần 7 và trình bày cuối kỳ kết hợp bàn giao vào tuần 15.

| Thời gian | Trọng tâm thực hiện | Kết quả dự kiến |
|---|---|---|
| **Tuần 1 đến 3** | Xác định yêu cầu, phạm vi và thiết kế tổng thể. | Thống nhất mục tiêu, module và phương án prototype. |
| **Tuần 4 đến 6** | Xây dựng prototype cho các nghiệp vụ chính. | Có phiên bản phục vụ trình diễn và đánh giá giữa kỳ. |
| **Tuần 7** | Đánh giá giữa kỳ theo yêu cầu đề bài. | Trình bày tiến độ, kết quả hiện có và các điểm cần điều chỉnh. |
| **Tuần 8 đến 11** | Hoàn thiện các module theo phạm vi đã xác định. | Prototype thể hiện đầy đủ các nhóm nghiệp vụ bắt buộc. |
| **Tuần 12 đến 14** | Kiểm thử, đối chiếu mục tiêu và hoàn thiện hồ sơ. | Ghi nhận kết quả kiểm chứng, sửa các điểm chưa đáp ứng và chuẩn bị bàn giao. |
| **Tuần 15** | Trình bày cuối kỳ và bàn giao dự án. | Bàn giao prototype cùng bộ tài liệu để nhóm khác tiếp nhận. |

### 3.1. Nhân sự và quản lý thực hiện
Đội dự án gồm 4 thành viên. Phương án phân công dưới đây xác định trách nhiệm theo vai trò trong quá trình triển khai UniSupport.

| Vai trò | Số người | Trách nhiệm chính |
|---|:---:|---|
| **Quản lý dự án kiêm phân tích nghiệp vụ** | 1 | Làm đầu mối trao đổi với khách hàng, làm rõ yêu cầu, quản lý phạm vi và tiến độ, điều phối nhóm và tổ chức nghiệm thu. |
| **Lập trình viên Backend** | 1 | Phát triển nghiệp vụ và dữ liệu của các module, phối hợp tích hợp hệ thống và xử lý lỗi thuộc phần phụ trách. |
| **Lập trình viên Frontend** | 1 | Thiết kế và phát triển giao diện website, bảo đảm cách trình bày phù hợp máy tính và thiết bị di động. |
| **Kiểm thử và tài liệu** | 1 | Chuẩn bị tình huống kiểm thử, đối chiếu kết quả với yêu cầu và hoàn thiện tài liệu phục vụ bàn giao. |

## 4. CHI PHÍ VÀ BÁO GIÁ DỰ ÁN

### 4.1. Giá chào trọn gói
Giá chào cho phạm vi UniSupport nêu trong proposal này bao gồm phân tích yêu cầu, thiết kế, phát triển các module, quản lý triển khai và bàn giao sản phẩm cùng tài liệu, là: **300.000.000 đồng** (Bằng chữ: Ba trăm triệu đồng).
Giá chào được xác định cho toàn bộ phạm vi thống nhất, không tách đơn giá từng chức năng. Yêu cầu bổ sung ngoài phạm vi được đánh giá và thống nhất riêng trước khi thực hiện.

### 4.2. Điều kiện thanh toán
Phương án thanh toán đề nghị được chia thành bốn đợt gắn với các mốc triển khai và kết quả bàn giao:

| Đợt | Mốc thanh toán | Tỷ lệ | Số tiền (VNĐ) |
|:---:|---|:---:|---:|
| **1** | Ký hợp đồng và khởi động dự án | 30% | 90.000.000 |
| **2** | Hoàn thành đánh giá giữa kỳ tuần 7 và được xác nhận | 30% | 90.000.000 |
| **3** | Hoàn thành nghiệm thu sản phẩm theo kế hoạch vào cuối tuần 14 | 30% | 90.000.000 |
| **4** | Hoàn tất bàn giao vào tuần 15 | 10% | 30.000.000 |
| **Tổng** | | **100%** | **300.000.000** |

## 5. QUẢN LÝ RỦI RO
Các rủi ro cần quản lý tập trung vào yêu cầu, tiến độ và khả năng đáp ứng đầu ra của dự án.

| Rủi ro | Ảnh hưởng | Hướng xử lý |
|---|---|---|
| **Yêu cầu thay đổi hoặc mâu thuẫn** | Phạm vi thiếu thống nhất, phải sửa lại sản phẩm. | Làm rõ yêu cầu với các bên liên quan, thống nhất ưu tiên trước khi điều chỉnh. |
| **Chậm tiến độ** | Không kịp mốc đánh giá giữa kỳ hoặc bàn giao. | Ưu tiên module bắt buộc, đánh giá phần việc còn lại và điều chỉnh kế hoạch khi có dấu hiệu chậm. |
| **Prototype chưa đáp ứng yêu cầu** | Không chứng minh được mục tiêu hoặc bị từ chối đầu ra. | Kiểm thử theo mục tiêu và phạm vi đã xác định, sửa các điểm chưa đạt trước khi trình bày. |
| **Tài liệu bàn giao chưa đầy đủ** | Nhóm khác không thể tiếp nhận dự án. | Đối chiếu tài liệu với prototype và kiểm tra khả năng tiếp nhận trước bàn giao. |

## 6. NGHIỆM THU VÀ BÀN GIAO
Nghiệm thu dựa trên mức độ đáp ứng phạm vi và mục tiêu đã thống nhất, có kết quả kiểm chứng bằng prototype và dữ liệu giả lập. Đầu ra chưa đạt tiêu chí cần được điều chỉnh trước khi xác nhận hoàn thành.

Bàn giao vào tuần 15 gồm prototype, mã nguồn, dữ liệu giả lập và tài liệu yêu cầu, thiết kế, kiểm thử, hướng dẫn sử dụng. Bộ tài liệu phải đủ rõ để một nhóm khác tiếp nhận và tiếp tục dự án.