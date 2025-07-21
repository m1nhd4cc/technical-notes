
## Quản lý User và Domain

**Di chuyển một domain sang user khác:**
```bash
cd /usr/local/directadmin/scripts
./move_domain.sh olddomain.com olduser newuser
```

**Cập nhật lại dung lượng sử dụng cho user (tally):**

- Cách 1: Chạy quota check
    ```bash
    /sbin/quotaoff -a; /sbin/quotacheck -augm; /sbin/quotaon -a;
    ```
- Cách 2: Thêm task vào hàng đợi của DA
    ```bash
    echo "action=tally&value=all" >> /usr/local/directadmin/data/task.queue
    ```

---

## Cấu hình và Thủ thuật

**Tìm mật khẩu root của MySQL:**
```bash
cat /usr/local/directadmin/conf/mysql.conf
```

**Đường dẫn quản lý package:**
- Admin packages: `/usr/local/directadmin/data/admin/packages/`
- User packages: `/usr/local/directadmin/data/users/admin/packages/`

> Lưu ý: Cần `chown diradmin:diradmin` cho các file và thư mục package sau khi chỉnh sửa.

**Cho phép cài SSL trên IP Shared (SNI):**
- Sửa file `/usr/local/directadmin/conf/directadmin.conf`.
- Thêm hoặc sửa dòng sau: `enable_ssl_sni=1`.
- Restart DirectAdmin:
    ```bash
    systemctl restart directadmin
    ```

**Thay đổi đường dẫn truy cập phpMyAdmin:**
- Tìm file alias của httpd, thường là `/etc/httpd/conf/extra/httpd-alias.conf`.
- Sửa dòng `Alias /phpmyadmin /var/www/html/phpMyAdmin` thành đường dẫn mong muốn.
- Restart httpd:
    ```bash
    systemctl restart httpd
    ```

**Cập nhật link trong giao diện DA:**
- Sửa các file HTML trong skin của DA:
    - `/usr/local/directadmin/data/skins/enhanced/user/db/db.html`
    - `/usr/local/directadmin/data/skins/enhanced/user/show_domain.html`

**Bật/Tắt chức năng đổi mật khẩu email trên Roundcube:**
- Sửa file `/usr/local/directadmin/conf/directadmin.conf`.
- Tìm `email_ftp_password_change`.
    - `1`: Bật
    - `0`: Tắt
- Restart DirectAdmin.

**Lỗi Roundcube: "DirectAdmin appears to be using SSL" khi đổi mật khẩu:**
- Sửa file `/var/www/html/roundcube/plugins/password/config.inc.php`.
- Thay đổi giá trị:
    ```php
    $config['password_directadmin_host'] = 'ssl://localhost';
    ```

---

## Bảo mật

**File chứa IP bị blacklist thủ công:**  
`/usr/local/directadmin/data/admin/ip_blacklist`

**Blacklist địa chỉ email người gửi:**  
Thêm địa chỉ email cần chặn vào file `/etc/virtual/blacklist_sender`.

**Tự động chặn IP Brute-force bằng CSF:**
- Sử dụng bộ script tùy chỉnh để tích hợp DA với CSF.
- Các file script (`block_ip.sh`, `brute_force_notice_ip.sh`,...) được đặt trong `/usr/local/directadmin/scripts/custom/`.
- Tắt thông báo brute-force của DA bằng cách thêm `hide_brute_force_notifications=1` vào `directadmin.conf`.

---

## Cập nhật và Bảo trì

**Swap IP (đổi IP chính của server):**
```bash
cd /usr/local/directadmin/scripts
./ipswap.sh OLD_IP NEW_IP
# Sau đó restart các dịch vụ mạng
systemctl restart httpd proftpd exim dovecot
```

**Kích hoạt license:**
```bash
cd /usr/local/directadmin/scripts
./getLicense.sh CLIENT_ID LICENSE_ID
systemctl restart directadmin
```

**Update mọi thành phần bằng CustomBuild:**
```bash
cd /usr/local/directadmin/custombuild
./build update
./build versions         # Kiểm tra phiên bản mới
./build update_versions  # Cập nhật toàn bộ hệ thống
```

**Upgrade một thành phần cụ thể (ví dụ PHP):**
```bash
cd /usr/local/directadmin/custombuild
# Sửa phiên bản mong muốn trong file options.conf
# vi options.conf
# php1_release=7.4
./build update
./build php n
./build rewrite_confs
```

**Xóa toàn bộ message/ticket hệ thống:**
```bash
cd /usr/local/directadmin/data/tickets
rm -rf *
```
