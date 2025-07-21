# Cài đặt extension `php-imap` cho CentOS 7

**Yêu cầu:** Cần quyền root để thực hiện.

## Bước 1: Cài đặt các gói phụ thuộc cần thiết
```bash
yum groupinstall "Development Tools" -y
yum install openssl-devel krb5-devel pam-devel -y
```

## Bước 2: Tải và biên dịch c-client

```bash
# Tạo thư mục làm việc
mkdir -p ~/php-imap-build && cd ~/php-imap-build

# Tải IMAP C-Client (ví dụ phiên bản 2007f)
wget ftp://ftp.cac.washington.edu/imap/imap-2007f.tar.gz
tar -xzvf imap-2007f.tar.gz
cd imap-2007f

# Sửa Makefile để biên dịch thư viện tĩnh
sed -i 's/EXTRACFLAGS= /EXTRACFLAGS=-fPIC /' Makefile

# Biên dịch
make lfd IP6=4
```

## Bước 3: Biên dịch extension imap.so từ source PHP

```bash
# Quay lại thư mục build
cd ~/php-imap-build

# Tải source code của phiên bản PHP đang dùng (kiểm tra bằng `php -v`)
# Ví dụ cho PHP 7.4
wget https://www.php.net/distributions/php-7.4.33.tar.gz
tar -xzvf php-7.4.33.tar.gz
cd php-7.4.33/ext/imap

# Chuẩn bị để biên dịch extension
phpize

# Cấu hình với đường dẫn đến thư mục c-client đã biên dịch
./configure --with-kerberos --with-imap-ssl --with-imap=$(realpath ~/php-imap-build/imap-2007f)

# Biên dịch và cài đặt
make
make install
```

## Bước 4: Kích hoạt extension

```bash
# Tìm đường dẫn đến file php.ini
php --ini

# Thêm dòng sau vào file php.ini
# extension=imap

# Hoặc tạo file config riêng
echo "extension=imap" > /etc/php.d/20-imap.ini

# Khởi động lại PHP-FPM và Web Server
systemctl restart php-fpm
systemctl restart httpd
```

## Bước 5: Kiểm tra

Tạo file `phpinfo.php` và kiểm tra xem mục imap đã xuất hiện chưa.

```php
<?php
phpinfo();
?>
