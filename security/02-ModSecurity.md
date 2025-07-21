## Thao tác với ModSecurity cho từng domain

**Khuyến nghị:** Nếu có thể, hãy dùng các công cụ tích hợp sẵn trên control panel (ví dụ: Comodo WAF trên DirectAdmin, Imunify360 trên cPanel).

**Cách làm thủ công (sửa file Virtual Host):**

1.  **Xác định file Virtual Host của domain:**
    -   Trên DirectAdmin: `/usr/local/directadmin/data/users/USERNAME/httpd.conf`
    -   Trên server Apache thường: `/etc/httpd/conf.d/vhosts/domain.conf` hoặc tương tự.
2.  Thêm các cấu hình bên dưới vào trong block `<VirtualHost ...>` của domain.

---

### Tắt ModSecurity cho một domain

```apache
<IfModule mod_security2.c>
    SecRuleEngine Off
</IfModule>
```

---

### Tắt một hoặc một vài Rule cụ thể

Hữu ích khi một rule hợp lệ nhưng lại chặn nhầm một chức năng của website.

```apache
<IfModule mod_security2.c>
    # Lấy ID của rule từ file log lỗi
    SecRuleRemoveById 212280
    SecRuleRemoveById 212650
    SecRuleRemoveById 212900
</IfModule>
```

---

### Tìm log lỗi của ModSecurity

Khi một yêu cầu bị chặn, thông tin sẽ được ghi vào log lỗi của domain.

```bash
# Tìm các dòng log chứa "ModSecurity" trong file lỗi của domain
tail -f /var/log/httpd/domains/yourdomain.com.error.log | grep 'ModSecurity'
```

Log sẽ chứa thông tin về rule đã kích hoạt, bao gồm cả ID của rule, ví dụ: `[id "212650"]`.

---

### Danh sách các ID ModSec cần Allow cho Web CNV

Nếu gặp lỗi trong "Cấu Hình Chung" của web CNV, thử allow các ID sau:

```
212000, 212620, 212700, 212740, 212870, 212890
