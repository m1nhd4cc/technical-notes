# ===================================================================
# LỆNH PHÂN QUYỀN
# ===================================================================
# Các lệnh này dùng để đặt lại quyền sở hữu và quyền truy cập
# cho các file và thư mục của một tài khoản hosting.
# Thay 'username' bằng tên người dùng cPanel.

# Set quyền cho thư mục là 751
find /home/username/public_html/ -type d -print0 | xargs -0 chmod 0751

# Set quyền cho file là 644
find /home/username/public_html/ -type f -print0 | xargs -0 chmod 0644

# Set quyền sở hữu
chown -R username:username /home/username/public_html/

# ===================================================================
# CẤU HÌNH
# ===================================================================

# Tăng giới hạn upload cho phpMyAdmin
# Sửa file: /usr/local/cpanel/3rdparty/php/53/etc/phpmyadmin/php.ini
# (Đường dẫn có thể thay đổi tùy phiên bản PHP)
#
# upload_max_filesize = 50M
# post_max_size = 50M

