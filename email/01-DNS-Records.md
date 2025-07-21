## Cấu hình DNS cho Google Workspace (G Suite)

### MX Records (Mail Exchanger)
Dùng để chỉ định mail server của Google sẽ nhận email cho domain của bạn.

| Tên/Host/Alias | TTL  | Loại | Ưu tiên | Giá trị/Trả lời/Đích             |
| :------------- | :--- | :--- | :------ | :-------------------------------- |
| `@` hoặc trống | 3600 | MX   | 1       | `ASPMX.L.GOOGLE.COM.`             |
| `@` hoặc trống | 3600 | MX   | 5       | `ALT1.ASPMX.L.GOOGLE.COM.`        |
| `@` hoặc trống | 3600 | MX   | 5       | `ALT2.ASPMX.L.GOOGLE.COM.`        |
| `@` hoặc trống | 3600 | MX   | 10      | `ALT3.ASPMX.L.GOOGLE.COM.`        |
| `@` hoặc trống | 3600 | MX   | 10      | `ALT4.ASPMX.L.GOOGLE.COM.`        |

### SPF Record (Sender Policy Framework)
Giúp ngăn chặn giả mạo email, xác thực rằng email được gửi từ server của Google.

- **Loại Record:** `TXT`
- **Tên/Host:** `@` hoặc để trống
- **Giá trị:** `v=spf1 include:_spf.google.com ~all`

### DKIM Record (DomainKeys Identified Mail)
Thêm một chữ ký điện tử vào email, giúp các server nhận mail xác thực nguồn gốc.

- **Loại Record:** `TXT`
- **Tạo trong Google Admin Console:** Apps -> Google Workspace -> Gmail -> Authenticate email.
- **Tên/Host:** `google._domainkey`
- **Giá trị:** (Sao chép giá trị được cung cấp trong Admin Console)

### CNAME Records cho các dịch vụ
Dùng để tạo các đường dẫn truy cập ngắn gọn cho các dịch vụ của Google.

| Tên/Host/Alias | Loại  | Giá trị/Đích             |
| :------------- | :---- | :----------------------- |
| `mail`         | CNAME | `ghs.googlehosted.com.`  |
| `drive`        | CNAME | `ghs.googlehosted.com.`  |
| `calendar`     | CNAME | `ghs.googlehosted.com.`  |
| `sites`        | CNAME | `ghs.googlehosted.com.`  |

---

## Cấu hình DNS cho Zoho Mail

### MX Records

| Tên/Host/Alias | TTL  | Loại | Ưu tiên | Giá trị/Trả lời/Đích |
| :------------- | :--- | :--- | :------ | :------------------ |
| `@` hoặc trống | 3600 | MX   | 10      | `mx.zoho.com.`      |
| `@` hoặc trống | 3600 | MX   | 20      | `mx2.zoho.com.`     |
| `@` hoặc trống | 3600 | MX   | 50      | `mx3.zoho.com.`     |

> **Lưu ý:** Các cấu hình SPF và DKIM cho Zoho Mail cũng cần được lấy từ trong trang quản trị của Zoho.

---

## Cấu hình DNS cho Blogspot (Custom Domain)

### A Records
Trỏ domain của bạn đến các server của Google.


```
216.239.32.21
216.239.34.21
216.239.36.21
216.239.38.21
```

> Bạn cần tạo 4 record A riêng biệt cho domain `@` (apex/root domain), mỗi record trỏ đến một IP trên.
