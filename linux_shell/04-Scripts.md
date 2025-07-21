#!/bin/bash
# ===================================================================
# SCRIPT LIỆT KÊ 10 THƯ MỤC LỚN NHẤT
# ===================================================================
# Cách dùng: ./script.sh /path/to/dir 10

du -hs * $1 |
sed 's/^\([0-9.,]*\)\([KMG]*\)/\2 \1\2/
     s/^K/1/;s/^M/2/;s/^G/3/' |
sort -g -k1 -k2n |
cut -c3- |
tail -$2


# SCRIPT BACKUP DATABASE AN TOÀN (DÙNG MYSQL_CONFIG_EDITOR)
# ========================================================
# Tránh lỗi "Warning: Using a password on the command line... can be insecure."

# Bước 1: Tạo một "login path" để lưu thông tin đăng nhập an toàn
# Lệnh này chỉ cần chạy 1 lần.
mysql_config_editor set --login-path=local --host=localhost --user=backup_user --password

# Bước 2: Sử dụng login path trong script backup
# Thay vì dùng -u và -p, dùng --login-path

DB_USER="backup_user"
DB_PASS="KHONG_CAN_DIEN_VAO_DAY"
BACKUP_DIR="/path/to/backups"
DB_NAME="my_database"

# Lệnh mysqldump sẽ tự động đọc thông tin từ login path 'local'
mysqldump --login-path=local --single-transaction $DB_NAME > $BACKUP_DIR/$DB_NAME-$(date +%F).sql


# SCRIPT TRANSFER FILE TỪ LINUX SANG WINDOWS (DÙNG CURL VÀ FTP)
# =============================================================

# Bước 1: Setup FTP Server trên Windows.
# Bước 2: Tạo file .netrc trên Linux để lưu thông tin đăng nhập FTP
# vi ~/.netrc
#
# Nội dung file:
# machine <FTP-Server-IP> login <ftp-user> password <ftp-password>
#
# Phân quyền chặt chẽ cho file
# chmod 0600 ~/.netrc

# Bước 3: Dùng curl trong script để upload
# curl --netrc --upload-file /path/to/local/file.zip ftp://<FTP-Server-IP>/

