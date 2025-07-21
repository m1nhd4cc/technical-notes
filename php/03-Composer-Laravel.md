# ===================================================================
# LỆNH COMPOSER & LARAVEL
# ===================================================================

# Di chuyển vào thư mục gốc của dự án Laravel
cd /home/davicneo/domains/davicneo.cnv.vn/public_html/system/

# Cập nhật các package (không bao gồm các package dev)
composer update --no-dev

# Nếu lệnh `composer` không tồn tại, có thể do chưa được cài đặt global.
# Cài đặt Composer theo hướng dẫn tại: [https://getcomposer.org/download/](https://getcomposer.org/download/)
# Sau đó chạy bằng lệnh sau:
php composer.phar update --no-dev

