# CẤU HÌNH REDIRECT HTTP SANG HTTPS

# Mở file cấu hình của domain (ví dụ: /etc/nginx/sites-enabled/domain.conf)
# và điều chỉnh lại block server cho port 80.

server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    # Chuyển hướng vĩnh viễn (301) tất cả truy cập HTTP sang HTTPS
    return 301 https://$server_name$request_uri;
}

# Sau khi sửa, cần khởi động lại Nginx
# service nginx restart

