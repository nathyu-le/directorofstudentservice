# Đo mục tiêu theo proposal

| Mã | Cách đo | Điều kiện đạt / cách đọc |
| --- | --- | --- |
| MET-01 | N yêu cầu mẫu trong vòng xử lý; M có đơn vị/người phụ trách hợp lệ và nhìn thấy được tại tiến độ; tỷ lệ M/N×100% | ≥90%; N phải >0, nêu thời điểm đo và mẫu; không loại bỏ hồ sơ khó để đạt tỷ lệ |
| MET-02 | N yêu cầu mẫu; K đọc được trạng thái hiện hành và đối chiếu khớp DB/lịch sử; K/N×100% | ≥90%; gồm cả chưa phân công/chờ bổ sung/kết thúc; N=0 là không đủ dữ liệu |
| MET-03 | Hai kịch bản cùng số yêu cầu, mức phức tạp và thời điểm diễn biến. B là lượt cần hỏi lại tiến độ cách cũ; A là lượt hỏi lại khi dùng prototype. Giảm=(B−A)/B×100% | ≥30%; B>0, ghi cách quan sát và giả định; B=0 không kết luận. Báo kết quả mô phỏng, không suy rộng vận hành thật |
| MET-04 | Cùng bộ request intent và khóa gửi; ghi số ý định bị bỏ sót, số hồ sơ dư do gửi lặp, đối chiếu hàng chờ/mã | Proposal chỉ nêu giảm bỏ sót/trùng, không có tỷ lệ cam kết; báo số trước/sau và E2E-07, không tự thêm KPI phần trăm |

Mẫu kết quả: mã phép đo, ngày/build/seed, N và tử số hoặc A/B, phép tính, evidence, kết luận (Chưa đo/Đạt/Chưa đạt/Không đủ dữ liệu), người thực hiện. Hiện toàn bộ **Chưa đo**. Mục tiêu G-05/G-06 dùng test báo cáo/quyền ở traceability; không thay bằng tỷ lệ mục tiêu của MET-01/02.
