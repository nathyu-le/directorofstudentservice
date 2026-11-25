# Dữ liệu và cách chạy kiểm thử (Mô phỏng)

Đây là đặc tả dữ liệu cho task BE-03 và QA.01. Trước khi chạy, đối chiếu ID thực tế của dữ liệu khởi tạo với bảng. Nếu trình nạp cấp ID khác, ghi bảng ánh xạ trong hồ sơ chạy và dùng ID đó trong payload/kiểm DB. Reset chỉ trên cơ sở dữ liệu test riêng trước mỗi case. Ghi build, cấu hình, phiên bản seed, actor, thời gian UTC, actual, evidence và bug/retest. Nếu chưa có ứng dụng thì giữ Not Run. Khi có lượt chạy nhưng thiếu điều kiện cụ thể, ghi Blocked và owner gỡ.

| Alias | ID đề xuất | Dữ liệu hoặc quyền |
| --- | --- | --- |
| SV-A, SV-B, SV-C | 101, 102, 103 | Student. SV-C không có hồ sơ. Email lần lượt sv-a@example.test, sv-b@example.test, sv-c@example.test. |
| NV-A, NV-B | 111, 112 | Staff, handler tại PB-A và PB-B. |
| DP-All, DP-A | 121, 122 | Staff có dispatch toàn trường và chỉ PB-A tương ứng. DP-A là biến thể phạm vi của QA. |
| QL-A, QL-All | 131, 132 | Manager có quyền report PB-A và toàn trường. Không cấp quyền đọc hồ sơ chi tiết mặc định. |
| Tài khoản ngừng | 199 | Bản sao cấu hình handler PB-A với active=false. |
| PB-A, PB-B, PB ngừng | 201, 202, 299 | Hai đơn vị hoạt động và một đơn vị không hoạt động. |
| Học vụ, CNTT, Chưa xác định, loại ngừng | 101, 102, 103, 199 | Bảng categories, ba loại hoạt động và một ngừng dùng. |

Email các actor còn lại dùng alias viết thường tại example.test. Mật khẩu thử lấy từ bộ khởi tạo của môi trường QA. Mỗi actor dùng phiên/cookie riêng. Trừ case CSRF, POST có phiên dùng mã CSRF hợp lệ. Thử trái quyền bằng request trực tiếp ở build thử để kiểm máy chủ, lưu body đã che bí mật.

| ID | Hồ sơ cơ sở |
| --- | --- |
| 310 | US-310, SV-A, Học vụ, Received, version 1, chưa giao/chưa có hạn. created_at 2026-12-01T01:00:00Z. |
| 311 | US-311, SV-A, PB-A/NV-A, WaitingInfo, version 4, câu hỏi 401 đang mở. started_at 2026-12-01T02:00:00Z, due_at 2026-12-03T01:00:00Z. |
| 312 | US-312, SV-A, PB-A/NV-A, WaitingInfo, version 4, câu hỏi 402 đang mở (khác hồ sơ 311). |
| 313 | US-313, SV-A, PB-A/NV-A, Resolved, version 6, kết quả 501, chưa có phản hồi. |
| 314 | US-314, SV-A, PB-A/NV-A, Closed, version 7, kết quả 502, chưa có phản hồi. |
| 315 | US-315, SV-A, PB-A/NV-A, Processing, version 3, có hạn và started_at, không câu hỏi mở. |
| 316 | US-316, SV-B, PB-B/NV-B, Processing, version 3. Dùng làm đối chứng khi lọc quyền. |
| 317 | US-317, SV-A, PB-A/NV-A, Received, version 2, không câu hỏi mở. Dùng để bắt đầu xử lý. |

Khi một case cần đổi trạng thái, quyền hoặc dữ liệu rỗng, tạo biến thể độc lập theo thao tác của case. Version dùng từ bản đã nạp, trừ case cũ hoặc sai. Với STU-01 dùng JSON cơ sở gồm category_id 101, title “Xin xác nhận sinh viên”, description “Xin hướng dẫn thủ tục”, submission_key UUID v4 mới. Ghi lại key, response và request_id. Dùng UUID mới cho các lần tạo độc lập. Với nhiều biến thể cùng case, lặp lại từ fixture mới nếu bước trước đã ghi dữ liệu.

## Hoàn tác và cập nhật đồng thời

Dùng điểm giả lập lỗi của build test ngay trước khi ghi history, sau khi nghiệp vụ đã ghi thay đổi trong giao dịch. BE cung cấp cách bật/tắt tại task self-check cùng endpoint. Chỉ chứng minh rollback khi log test xác nhận đã qua bước ghi dữ liệu chính và lỗi xảy ra trước commit. Tắt giả lập sau lượt lỗi, đọc lại bằng kết nối khác và so sánh chính xác hồ sơ, record_version, các bảng con và history trong phạm vi case. Nếu chưa có điểm giả lập được mô tả, ghi Blocked và chuyển BE.

Với phiên bản cũ, giữ hai bản sao payload có cùng version. Gửi A hợp lệ, đợi xác nhận và chụp dữ liệu đã lưu. Gửi B bằng version cũ, đối chiếu 409 và ảnh chụp sau A. Với trường hợp mất phản hồi lúc tạo, xác nhận commit từ môi trường test trước khi bỏ response phía client, rồi gửi lại cùng UUID và payload. Đây là tình huống khác lỗi trước commit.

## Nguồn tính báo cáo

Tạo tập độc lập cho mỗi báo cáo với PB-A và đối chứng PB-B. Chốt đồng hồ thử UTC. Bộ lọc ngày một ngày 02/12/2026 theo giờ Việt Nam có khoảng UTC từ 01/12 17:00:00 (gồm) đến 02/12 17:00:00 (không gồm). Case biên dùng đúng hai mốc này và một giây trước/sau. Nếu lọc kỳ, đặt mốc trên đúng cột nguồn.

| Báo cáo | Tập kiểm độc lập và phép tính |
| --- | --- |
| RPT-01 | Hai hồ sơ PB-A tạo trong kỳ, một Received, một Processing. Đối chứng: PB-B và ngoài kỳ. Dùng requests.created_at, gom status hiện tại. Mẫu 2, ba nhóm còn lại đếm 0. |
| RPT-02 | Chốt as_of 2026-12-02T06:00:00Z. Hạn của ba hồ sơ mở có thể là 03:00, 03:00 và 05:00. Đối chứng hạn bằng 06:00, không hạn và Closed. Dùng requests.due_at, mẫu 3, số giờ lần lượt 3, 3, 1. |
| RPT-03 | Hai hồ sơ PB-A giải quyết trong kỳ, khoảng thời gian 2 và 6 giờ tính từ created_at. Một hồ sơ đã chờ bổ sung, một hồ sơ ngoài kỳ. Dùng resolved_at, mẫu 2, trung bình 4, min 2, max 6. |
| RPT-04 | Hai hồ sơ Học vụ và một CNTT ở PB-A trong kỳ. Dùng created_at, category_id hiện tại. Mẫu 3. Với case hòa đổi thành một hồ sơ mỗi nhóm và so category_id tăng dần. |
| RPT-05 | Hai hồ sơ mở PB-A (NV-A), một mở PB-B (NV-B), một chưa phân công và một Closed. Không lọc ngày. QL-All thấy 4 hồ sơ mở, QL-A chỉ thấy 2 của PB-A. |
| RPT-06 | Hai phản hồi PB-A trong kỳ điểm 4 và 2, một hồ sơ chưa đánh giá. Dùng feedback.updated_at, mẫu 2, mean 3, các nhóm điểm 2 và 4 mỗi nhóm 1. Case đổi kỳ đặt updated_at qua biên, giữ nguyên feedback_id. |

Với case chống đếm trùng, thêm hai dòng lịch sử và hai ghi chú cho cùng hồ sơ mà không thêm hồ sơ/phản hồi, lấy lại báo cáo và đối chiếu tập mã, mẫu số và phép tính trước/sau. Với case sai quyền, gửi bộ lọc PB-B bằng QL-A, đối chiếu rằng phản hồi không chứa số tổng hoặc mã hồ sơ. Tên bảng, trường và đường dẫn chi tiết xem [từ điển dữ liệu](../02-domain/data-dictionary.md) và [hợp đồng API](../02-domain/api-contract.md).
