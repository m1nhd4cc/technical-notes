## Lỗi: An unexpected database error occurred

**Nguyên nhân & Giải pháp:**

1.  **Sai chuỗi kết nối:** Kiểm tra lại `host`, `username`, `password`, `dbname` trong file config của website.
2.  **Database bị lỗi:**
    -   Đăng nhập vào phpMyAdmin.
    -   Chọn database bị lỗi.
    -   Chọn tất cả các bảng (Check All).
    -   Ở menu "With selected:", chọn "Repair table".
3.  **MySQL Server quá tải hoặc timeout:**
    -   Tăng các giá trị trong file `my.cnf`:
        ```ini
        wait_timeout = 600
        connect_timeout = 120
        max_allowed_packet = 64M
        max_connections = 800
        ```
    -   Khởi động lại dịch vụ MySQL.

---

## Lỗi: Access Denied khi vào PHPMyAdmin - Unable to establish a PHP session

**Nguyên nhân & Giải pháp:**

1.  **Sai phân quyền thư mục `tmp`:**
    -   Thư mục `tmp` dùng để lưu session cần có quyền ghi cho user web server.
    -   Trên hosting, đảm bảo thư mục `tmp` trong `public_html` (hoặc thư mục được cấu hình trong `php.ini`) có quyền `755`.
2.  **Hosting đầy dung lượng:**
    -   Kiểm tra dung lượng hosting. Nếu đầy, xóa bớt các file không cần thiết (log, cache, backup cũ).

---

## Lỗi: VPS/Server không start được hoặc MySQL không start lên

**Nguyên nhân & Giải pháp:**

-   **Hết dung lượng ổ cứng:** Đây là nguyên nhân phổ biến nhất. MySQL cần không gian trống để ghi log và các file tạm.
    -   Dùng lệnh `df -h` để kiểm tra dung lượng còn lại của các phân vùng.
    -   Nếu đầy, tìm và xóa các file lớn, không cần thiết.

---

## Lỗi Log: Error in accept: Too many open files

**Nguyên nhân:**  
Hệ điều hành đã đạt đến giới hạn số lượng file được phép mở cùng lúc, và MySQL là một trong những tiến trình bị ảnh hưởng.

**Giải pháp:** Tăng giới hạn `open files`.

1.  **Kiểm tra giới hạn hiện tại:**
    ```bash
    ulimit -a
    ```
2.  **Tăng giới hạn vĩnh viễn:**
    -   Sửa file `/etc/security/limits.conf` và thêm vào cuối:
        ```
        * soft   nofile   65536
        * hard   nofile   65536
        ```
3.  **Cấu hình cho MySQL:**
    -   Sửa file `/etc/my.cnf` và thêm vào mục `[mysqld]`:
        ```ini
        open_files_limit = 65536
        ```
4.  **Khởi động lại server hoặc session để áp dụng.**

---

## Lỗi mysqldump: Got error: 1044: Access denied ... when using LOCK TABLES

**Nguyên nhân:**  
User dùng để dump database không có quyền `LOCK TABLES` trên tất cả các bảng (đặc biệt là các bảng hệ thống như `information_schema`).

**Giải pháp:**  
Sử dụng tùy chọn `--single-transaction` để tạo một bản dump nhất quán mà không cần khóa bảng.

```bash
mysqldump --single-transaction -u user -p DBNAME > backup.sql
```

> **Lưu ý:** Tùy chọn này chỉ hoạt động hiệu quả với các storage engine hỗ trợ transaction như InnoDB.

---

## Lỗi MariaDB 10.4+: Column 'authentication_string' / 'Password' is not updatable

**Nguyên nhân:**  
Từ MariaDB 10.4, bảng `mysql.user` đã trở thành một VIEW (khung nhìn) và không thể cập nhật trực tiếp được nữa.

**Giải pháp:**  
Sử dụng lệnh `SET PASSWORD` để thay đổi mật khẩu.

```sql
-- Đăng nhập với quyền root
SET PASSWORD FOR 'someuser'@'localhost' = PASSWORD('newpassword');
-- Hoặc nếu đổi mật khẩu cho user hiện tại
SET PASSWORD = PASSWORD('newpassword');
```
