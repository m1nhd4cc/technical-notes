# Một số vấn đề thường gặp & cách khắc phục trên Web Server

## 1. Trang bảo trì đơn giản

Sử dụng file `index.html` với nội dung sau để hiển thị thông báo bảo trì:

```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Website Đang Bảo Trì</title>
    <style>
        body { text-align: center; padding: 50px; font-family: sans-serif; }
        h1 { font-size: 40px; }
        p { font-size: 20px; color: #333; }
    </style>
</head>
<body>
    <img src="https://.../under-construction.jpg" alt="Website đang được bảo trì" width="400">
    <h1>Chúng tôi sẽ sớm quay trở lại!</h1>
    <p>Xin lỗi vì sự bất tiện này, chúng tôi đang thực hiện bảo trì. Vui lòng quay lại sau.</p>
</body>
</html>
```

---

## 2. Lỗi 503 Forbidden hoặc Service Unavailable

### Kiểm tra file cấu hình Apache

- Kiểm tra file `/etc/httpd/conf/httpd.conf`.
- Nếu nghi ngờ file bị lỗi, có thể phục hồi từ bản backup hoặc build lại.

### Kiểm tra phân quyền

- Nếu chỉ một vài website bị lỗi, nguyên nhân lớn là do sai phân quyền thư mục và file.
- Đảm bảo user của web server (vd: `apache`, `www-data`) có quyền đọc và thực thi.

---

## 3. ProFTPD: Login thành công nhưng không liệt kê được thư mục

**Nguyên nhân:** Do firewall chặn các port dành cho Passive mode của FTP.

**Giải pháp:**  
Mở dải port `35000:36000` (hoặc dải port được cấu hình trong `proftpd.conf`) trên firewall của server.

---

## 4. Chặn truy cập bằng IP trên Apache (VPS)

Để tránh nội dung trùng lặp hoặc truy cập không mong muốn, cấu hình để IP server chỉ hiển thị một trang trống.

### Chỉnh sửa file Virtual Host mặc định (`000-default.conf`):

```apache
<VirtualHost *:80>
    ServerName 14.161.27.76
    DocumentRoot /var/www/html/ # Thư mục này chứa 1 file index.html trống
    <Directory /var/www/html/>
        AllowOverride None
        Require all denied
    </Directory>
</VirtualHost>
```

### Tạo Virtual Host riêng cho domain:

```apache
<VirtualHost *:80>
    ServerName yourdomain.com
    ServerAlias www.yourdomain.com
    DocumentRoot /var/www/yourdomain_source/
    # ... các cấu hình khác
</VirtualHost>
```

### Kích hoạt site và khởi động lại Apache:

```bash
a2ensite yourdomain.com.conf
systemctl reload apache2
```
