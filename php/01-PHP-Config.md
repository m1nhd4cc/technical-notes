# CÁC CẤU HÌNH PHP.INI THÔNG DỤNG
# ===============================

# Đường dẫn lưu session
session.save_path = "/home/user/public_html/tmp"

# Hiển thị lỗi (Bật khi dev, tắt khi production)
display_errors = On
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT

# Giới hạn tài nguyên
memory_limit = 256M
post_max_size = 100M
upload_max_filesize = 100M
max_execution_time = 300
max_input_time = 300
max_allowed_packet = 50M # Cũng cần set trong my.cnf

# Cấu hình bộ đệm
output_buffering = On

# Timezone
date.timezone = "Asia/Ho_Chi_Minh"

# Vô hiệu hóa các hàm nguy hiểm
disable_functions = system,exec,shell_exec,passthru,pcntl_exec,proc_close,proc_get_status,proc_nice,proc_open,proc_terminate,escapeshellcmd,escapeshellarg,dl,show_source,ini_alter,virtual,openlog,mail,symlink

# Bật nén Gzip
zlib.output_compression = On

# Các cấu hình cũ (có thể không còn cần thiết)
safe_mode = Off
register_globals = Off
magic_quotes_gpc = Off

# XỬ LÝ SỰ CỐ LIÊN QUAN ĐẾN PHP.INI
# =================================

### Lỗi file_put_contents không hoạt động
# Đảm bảo output_buffering được bật
output_buffering = On

### Lưu ý khi setup php.ini trên các loại Server API khác nhau
# Sau khi sửa php.ini, cần restart đúng dịch vụ:
# - Server API là Apache 2.0 Handler:
#   systemctl restart httpd.service
# - Server API là FPM/FastCGI (ví dụ PHP 7.4):
#   systemctl restart php74-php-fpm.service

### Xử lý disable_function riêng cho từng website trên DirectAdmin (dùng Suhosin)
# Cách này cho phép mở một hàm bị chặn cho một website cụ thể.

# 1. Cài đặt và kích hoạt suhosin extension.
# 2. Trong php.ini của server, xóa danh sách hàm trong `disable_functions`.
# 3. Thay vào đó, dùng `suhosin.executor.func.blacklist`:
#    suhosin.executor.func.blacklist = exec,shell_exec,passthru,system
# 4. Trong file cấu hình riêng của user (vd: /usr/local/directadmin/data/users/USERNAME/php/php-fpm74.conf), thêm dòng sau để "whitelist" một hàm:
#    php_admin_value[suhosin.executor.func.whitelist] = shell_exec
# 5. Restart PHP-FPM service.

