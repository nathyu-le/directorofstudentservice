# Yêu cầu Phi chức năng (Non-Functional Requirements)

## 1. Công nghệ & Môi trường
- **Backend:** PHP.
- **Database:** MySQL.
- **Frontend:** HTML/CSS/JS (Responsive hỗ trợ giao diện Mobile & PC).

## 2. Bảo mật (Security)
- Mật khẩu lưu trong Database phải được mã hóa (hashing như Bcrypt), không lưu plain-text.
- Bảo vệ các endpoint chống SQL Injection và XSS bằng cách sử dụng Prepared Statements và sanitize đầu vào.

## 3. Hiệu năng & Khả năng sử dụng (Usability)
- Prototype phải có khả năng xử lý mượt mà bộ dữ liệu giả lập (ít nhất 500 records) mà không bị treo.
- Giao diện trực quan, hạn chế tối đa số lần click chuột để hoàn thành một tác vụ.