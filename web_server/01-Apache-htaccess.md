# ===================================================================
# CẤU HÌNH REWRITE CƠ BẢN
# ===================================================================

### Cấu hình rewrite cho Joomla
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
</IfModule>

### Cấu hình rewrite cho Wordpress
# BEGIN WordPress
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
</IfModule>
# END WordPress

### Cấu hình rewrite vào thư mục con
# Yêu cầu: chuyển hướng truy cập từ domain.com sang domain.com/blog
RewriteEngine On
RewriteCond %{HTTP_HOST} ^vietcoding.com$ [OR]
RewriteCond %{HTTP_HOST} ^www.vietcoding.com$
RewriteCond %{REQUEST_URI} !^/blog
RewriteRule (.*) /blog/$1 [L]

### Lỗi Subdomain không chạy được file index.php
# Nguyên nhân: do cấu hình rewrite không chính xác.
# Dòng lỗi ví dụ:
RewriteRule ^([^\.]+)\/$ index.php?id=$1 [NC]


# ===================================================================
# BỘ SƯU TẬP CẤU HÌNH .HTACCESS
# ===================================================================

### Set trang mặc định (thay cho index.php/html)
DirectoryIndex info.html

### Thêm "www" vào URL
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} !^www\.
RewriteRule ^(.*)$ http://www.%{HTTP_HOST}/$1 [R=301,L]

### Bỏ "www" ra khỏi URL
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
RewriteRule ^(.*)$ http://%1/$1 [R=301,L]

### Chuyển hướng sang HTTPS
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

### Redirect đến trang lỗi tùy chỉnh
ErrorDocument 401 /error/401.php
ErrorDocument 403 /error/403.php
ErrorDocument 404 /error/404.php
ErrorDocument 500 /error/500.php

### Redirect 301 (Tốt cho SEO)
# Redirect một trang cụ thể
Redirect 301 /old/old-page.html http://yourdomain.com/new-page.html

# Redirect toàn bộ domain
RewriteEngine On
RewriteRule ^(.*)$ http://newdomain.com/$1 [R=301,L]

# Redirect tên miền cụ thể vào thư mục con
RewriteEngine On
RewriteCond %{HTTP_HOST} ^luatsunguyenle.vn$
RewriteRule ^$ http://luatsunguyenle.vn/suspended/ [L,R=301]

### Bỏ đuôi file .php
RewriteRule ^(([^/]+/)*[^.]+)$ /$1.php [L]

### Thêm dấu "/" vào cuối URL
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_URI} !#
RewriteCond %{REQUEST_URI} !(.*)/$
RewriteRule ^(.*)$ http://yourdomain.com/$1/ [L,R=301]

### Chuyển dấu "_" thành "-" trong URL
Options +FollowSymLinks
RewriteEngine On
RewriteBase /
RewriteRule !\.(html|php)$ - [S=4]
RewriteRule ^([^_]*)_([^_]*)_([^_]*)_([^_]*)_(.*)$ $1-$2-$3-$4-$5 [E=uscor:Yes]
RewriteRule ^([^_]*)_([^_]*)_([^_]*)_(.*)$ $1-$2-$3-$4 [E=uscor:Yes]
RewriteRule ^([^_]*)_([^_]*)_(.*)$ $1-$2-$3 [E=uscor:Yes]
RewriteRule ^([^_]*)_(.*)$ $1-$2 [E=uscor:Yes]
RewriteCond %{ENV:uscor} ^Yes$
RewriteRule (.*) http://yourdomain.com/$1 [R=301,L]

# ===================================================================
# BẢO MẬT VỚI .HTACCESS
# ===================================================================

### Chặn hotlink hình ảnh
Options +FollowSymlinks
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)?yourdomain.com/ [NC]
RewriteRule \.(gif|jpg|png)$ http://yourdomain.com/images/nohotlink.gif [NC,L]

### Chặn truy cập theo địa chỉ IP
Order allow,deny
Allow from all
Deny from 192.168.1.123
Deny from 113.161.86.133

### Chỉ cho phép 1 IP truy cập
Order deny,allow
Deny from all
Allow from 113.161.86.133

### Bảo vệ các file nhạy cảm
<files .htaccess>
    Order allow,deny
    Deny from all
</files>
<files wp-config.php>
    Order allow,deny
    Deny from all
</files>

### Đặt mật khẩu cho thư mục
AuthType Basic
AuthName "This directory is protected"
AuthUserFile /home/path/to/.htpasswd
Require valid-user

### Đặt mật khẩu cho file
<Files "secure.php">
    AuthType Basic
    AuthName "Prompt"
    AuthUserFile /home/path/to/.htpasswd
    Require valid-user
</Files>

# ===================================================================
# CẤU HÌNH KHÁC
# ===================================================================

### Cho phép Cross-Origin Resource Sharing (CORS)
# Cho phép website khác sử dụng tài nguyên (font, css, js) từ site của bạn
<IfModule mod_headers.c>
  <FilesMatch "\.(ttf|ttc|otf|eot|woff|woff2|font.css|css|js)$">
    Header set Access-Control-Allow-Origin "*"
  </FilesMatch>
</IfModule>

### Enable method PUT, DELETE...
# Cần quyền admin server để cấu hình trong httpd.conf
# Hoặc trên DirectAdmin:
# cd /usr/local/directadmin/custombuild
# ./build set http_methods GET:HEAD:POST:PUT:DELETE:PATCH
# ./build rewrite_confs

