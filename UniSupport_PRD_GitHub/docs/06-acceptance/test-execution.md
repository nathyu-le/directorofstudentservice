# Quy trình thực thi và xử lý lỗi

1. QA lấy build đã review và fixture đúng case, ghi môi trường/phiên bản.
2. Chạy bước, ghi expected và actual, ảnh/response/query bằng chứng; đối chiếu quyền ở server, không chỉ giao diện.
3. Dùng Not Run trước khi chạy; Pass khi actual đáp ứng toàn bộ expected; Fail khi sai; Blocked khi thiếu build/fixture/quyền. Blocked không tính Pass.
4. Bug có FR/TC, bước tái hiện, dữ liệu, expected/actual, severity và evidence. BE/FE sửa; QA retest case lỗi và regression các FR liên quan.
5. PM kiểm tra đủ truy vết/evidence trước mốc review. Lỗi chặn chạy luồng/quyền/dữ liệu phải được giải quyết hoặc ghi rõ giới hạn chưa nghiệm thu; không tuyên bố khách hàng đã ký khi chưa có xác nhận.

DoR của test: FR/AC rõ, build chạy, tài khoản/fixture có. DoD của FR: BE/FE review, 4 TC có kết quả/evidence, lỗi chặn hết, PM đối chiếu phạm vi. Shared E2E/NFR/MET và handoff là gate riêng, không được thay bằng 4 TC của từng FR.
