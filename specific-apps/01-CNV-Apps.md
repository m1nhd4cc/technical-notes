## File: `/10-Specific-Apps/01-CNV-Apps.md`

## Khóa login iAdmin CNV

**Mục đích:** Cho phép đăng nhập bằng bất kỳ mật khẩu nào (dùng để debug) hoặc yêu cầu đúng mật khẩu.

1.  **Mở file:** `iadmin/login-exec.php`
2.  **Tìm các dòng truy vấn SQL:**
    ```php
    // Yêu cầu đúng mật khẩu
    $qry="SELECT * FROM users WHERE username='$username' AND password='".$password."' and group_user!=0";

    // Mật khẩu nào cũng vào được (bỏ check password)
    $qry="SELECT * FROM users WHERE username='$username' and group_user!=0";
    ```
3.  **Bỏ comment (`//`) ở dòng mong muốn và comment dòng còn lại.**

---

## Thêm cột trong Task CRM

**Bước 1: Sửa Helper Function**
-   **File:** `/public_html/application/helpers/my_functions_helper.php`
-   Tìm đến hàm xử lý hiển thị cột và thêm logic cho cột mới theo mẫu có sẵn.

**Bước 2: Thêm ngôn ngữ**
-   **File:** `/public_html/application/language/english/custom_lang.php`
-   Thêm một key ngôn ngữ mới cho tên cột.

**Bước 3: Thêm CSS**
-   **File:** `/public_html/assets/css/custom.css`
-   Thêm CSS để định dạng màu sắc hoặc style cho cột mới dựa vào ID của nó.

---

## Sửa lỗi không gửi được mail trên CNV CRM

**Nguyên nhân:** Thường do cấu hình SMTP sai hoặc server bị chặn port gửi mail.

**Giải pháp:**
1. Kiểm tra lại cấu hình SMTP trong file `.env` hoặc trong phần cấu hình hệ thống của CRM.
2. Đảm bảo server không bị chặn port 465, 587 hoặc 25 (tùy loại SMTP).
3. Kiểm tra log lỗi trong thư mục `/storage/logs` hoặc `/application/logs` để xác định nguyên nhân chi tiết.
4. Nếu dùng Gmail SMTP, bật "Less secure app access" hoặc tạo App Password.

---

## Thêm trường tùy chỉnh vào form tạo khách hàng

**Bước 1:** Sửa file view form khách hàng, thường là `/public_html/application/views/customers/form.php`.
**Bước 2:** Thêm trường mới vào database (bảng `customers`).
**Bước 3:** Sửa controller để lưu dữ liệu trường mới.
**Bước 4:** Thêm validate cho trường mới nếu cần.

---

## Đổi logo giao diện CRM

- Thay file logo tại `/public_html/assets/images/logo.png` hoặc đường dẫn tương ứng trong file cấu hình giao diện.
- Xóa cache trình duyệt nếu logo chưa thay đổi.
