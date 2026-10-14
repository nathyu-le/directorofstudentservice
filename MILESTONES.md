# Lộ trình dự án UniSupport (Milestones)

Dưới đây là kế hoạch triển khai tổng thể trong 15 tuần theo biểu đồ Gantt:

```mermaid
gantt
    title Kế hoạch Triển khai Dự án UniSupport (15 Tuần)
    dateFormat  YYYY-MM-DD
    axisFormat  %d/%m
    
    section GĐ1: Khởi tạo (Tuần 1-3)
    Phân tích Yêu cầu & Viết PRD      :a1, 2026-09-07, 10d
    Thiết kế DB Schema & UX/UI        :a2, after a1, 11d
    
    section GĐ2: Phát triển đợt 1 (Tuần 4-6)
    M04 - Auth & Phân quyền           :b1, 2026-09-28, 10d
    M01 - Cổng Sinh Viên              :b2, after b1, 11d
    
    section Mốc Giữa Kỳ (Tuần 7)
    Báo cáo Tiến độ (Demo M04, M01)   :milestone, m1, 2026-10-19, 0d
    Đánh giá & Điều chỉnh             :c1, 2026-10-19, 7d
    
    section GĐ3: Phát triển đợt 2 (Tuần 8-11)
    M02 - Điều phối & Xử lý (Staff)   :d1, 2026-10-26, 14d
    M03 - Dashboard Quản lý (Manager) :d2, after d1, 14d
    
    section GĐ4: Kiểm thử (Tuần 12-14)
    Chạy Test Cases & Kiểm tra        :e1, 2026-11-23, 10d
    Sửa lỗi (Fix bugs) & Mock Data    :e2, after e1, 11d
    Code Freeze (Đóng băng code)      :milestone, m2, 2026-12-13, 0d
    
    section Bàn giao (Tuần 15)
    Chuẩn bị hồ sơ nghiệm thu         :f1, 2026-12-14, 5d
    Trình bày & Bàn giao cuối kỳ      :milestone, m3, 2026-12-19, 0d