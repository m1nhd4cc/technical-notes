-- ===================================================================
-- QUẢN LÝ USER VÀ QUYỀN
-- ===================================================================

-- Tạo user mới
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';

-- Cấp toàn bộ quyền trên một database cho user
GRANT ALL PRIVILEGES ON `database_name`.* TO 'newuser'@'localhost';

-- Cấp quyền cho user kết nối từ bất kỳ IP nào
GRANT ALL PRIVILEGES ON `database_name`.* TO 'newuser'@'%' IDENTIFIED BY 'password';

-- Giới hạn số lượng kết nối/truy vấn mỗi giờ
GRANT USAGE ON `database_name`.* TO 'db_user'@'localhost'
    WITH MAX_QUERIES_PER_HOUR 1000
    MAX_UPDATES_PER_HOUR 100
    MAX_CONNECTIONS_PER_HOUR 120;

-- Xem user có thể kết nối từ đâu
SELECT User, Host FROM mysql.user WHERE User = 'cnvsoft';

-- Xóa user
DROP USER 'username'@'localhost';

-- Áp dụng thay đổi quyền
FLUSH PRIVILEGES;


-- ===================================================================
-- QUẢN LÝ MẬT KHẨU
-- ===================================================================

-- Đổi mật khẩu cho user 'root' (khi đang đăng nhập)
-- Dành cho MySQL 5.7+ và MariaDB 10.1.20+
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';

-- Dành cho phiên bản cũ hơn
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('new_password');

-- Dành cho MariaDB 10.4+ (khi user là user hiện tại)
SET PASSWORD = PASSWORD('new_password');


-- Đổi mật khẩu root khi bị quên
/*
1. Stop MySQL:
   # service mysqld stop
2. Start MySQL ở chế độ bỏ qua bảng phân quyền:
   # mysqld_safe --skip-grant-tables &
3. Đăng nhập không cần mật khẩu:
   # mysql -u root
4. Chạy các lệnh SQL sau:
   FLUSH PRIVILEGES;
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
   \q
5. Khởi động lại MySQL bình thường:
   # service mysqld start
*/

-- Xem mật khẩu root được lưu trong config của DirectAdmin
-- cat /usr/local/directadmin/conf/mysql.conf

-- Xem mật khẩu root được lưu trong file my.cnf của user root
-- cat /root/.my.cnf


-- ===================================================================
-- QUẢN LÝ DATABASE VÀ TABLE
-- ===================================================================

-- Tạo database
CREATE DATABASE new_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Xóa database
DROP DATABASE database_name;

-- Xem danh sách database
SHOW DATABASES;

-- Chọn một database để làm việc
USE database_name;

-- Xem danh sách các bảng trong database hiện tại
SHOW TABLES;

-- Update dữ liệu trong một bảng
-- Ví dụ: thay thế một chuỗi trong cột 'thumbnail'
UPDATE posts SET thumbnail = REPLACE(thumbnail, 'comstorage', 'com/storage');


-- ===================================================================
-- BACKUP & RESTORE
-- ===================================================================

-- Export (dump) một database
-- mysqldump -u [user] -p [database_name] > backup.sql

-- Export tất cả database
-- mysqldump -u root -p --all-databases > all_databases.sql

-- Import một database
-- mysql -u [user] -p [database_name] < backup.sql

-- Import dữ liệu từ file .sql khi đang trong mysql client
-- USE database_name;
-- SOURCE /path/to/backup.sql;


-- ===================================================================
-- CHẾ ĐỘ READ-ONLY (DÙNG KHI CHUYỂN SERVER)
-- ===================================================================

-- Khóa tất cả các bảng và bật chế độ chỉ đọc
FLUSH TABLES WITH READ LOCK;
SET GLOBAL read_only = 1;

-- Mở lại chế độ ghi
SET GLOBAL read_only = 0;
UNLOCK TABLES;

