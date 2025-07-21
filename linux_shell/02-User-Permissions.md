# QUẢN LÝ USER

### Cho phép một user (không phải root) đăng nhập SSH
# 1. Sửa file cấu hình SSHD:
#    vi /etc/ssh/sshd_config
# 2. Thêm vào cuối file:
#    AllowUsers username1 username2
# 3. Restart SSHD:
#    systemctl restart sshd

### Đổi shell mặc định cho user
# Dùng để cho phép user sử dụng các lệnh bash.
# 1. Xem shell hiện tại:
#    echo $SHELL
# 2. Đổi shell cho user:
#    chsh -s /bin/bash username

### Xử lý lỗi "Unable to remove Unix User" trên control panel
# Nguyên nhân thường do file mailbox của user có quyền sở hữu không đúng.
# cd /var/spool/mail/
# chown username:mail username

# PHÂN QUYỀN FILE VÀ THƯ MỤC
# ==========================

### Phân quyền cứng (immutable) để chống sửa/xóa file
# Lệnh này làm cho file không thể bị thay đổi, ngay cả bởi user root.
#
# Khóa file .htaccess:
chattr +i /home/user/public_html/.htaccess

# Mở khóa file .htaccess:
chattr -i /home/user/public_html/.htaccess

### Tạo shortcut (symbolic link)
# ln -s [Đường dẫn file gốc] [Tên shortcut]
ln -s /usr/local/php74/bin/php php74
# Lệnh trên sẽ tạo một file tên là `php74` tại thư mục hiện hành,
# trỏ đến file thực thi của PHP 7.4.

