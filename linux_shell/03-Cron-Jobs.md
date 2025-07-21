## Cấu trúc một dòng Cron Job

```
* * * * * command to be executed
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)
```

**Ví dụ:**
-   `*/5 * * * *` : Chạy mỗi 5 phút.
-   `0 2 * * *` : Chạy vào 2 giờ sáng mỗi ngày.
-   `0 4 * * 1` : Chạy vào 4 giờ sáng mỗi thứ Hai.

---

## Ví dụ về Cron Job

### Tạo cron tìm và xóa file `core.xxxx` trong WordPress

1.  Mở file cron của root:
    ```bash
    crontab -e
    # Hoặc vi /var/spool/cron/root
    ```
2.  Thêm dòng sau để chạy mỗi 5 phút:
    ```cron
    */5 * * * * find /home/*/public_html/ -type f -name "core.[0-9]*" -print0 | xargs -0 rm
    ```
3.  Restart dịch vụ cron:
    ```bash
    systemctl restart crond
    ```

---

### Cron chạy một file PHP

```cron
# Chạy bằng command line PHP
*/15 * * * * /usr/local/bin/php -q /home/user/public_html/cron.php > /dev/null 2>&1

# Chạy bằng cách gọi URL (cần cài wget)
*/15 * * * * /usr/bin/wget -q -O- https://yourdomain.com/cron.php > /dev/null 2>&1
```

> `/dev/null 2>&1` dùng để không gửi email thông báo kết quả sau mỗi lần chạy.

---

### Cron và Email Piping (Ví dụ CRM)

Đây là một ví dụ phức tạp dùng để pipe email vào một script PHP thông qua SSH.

**Giá trị forwarder trong cPanel:**

```
"|sshpass -p 'YourSshPassword' ssh -p 22999 -o StrictHostKeyChecking=no root@103.53.168.230 '/usr/bin/php -q /home/crm/crm.cnv.vn/pipe.php' >
```
