## Quản lý hàng đợi mail Exim

**Xem thống kê hàng đợi:**
```bash
exim -bpc
```

Hoặc xem chi tiết hơn:
```bash
/usr/sbin/exim -bp | awk '{print $4}' | sed '/^$/ d' | sort | uniq -c
```

**Xóa mail khỏi hàng đợi:**

```bash
# Xóa tất cả mail trong hàng đợi (CẨN THẬN!)
/usr/sbin/exim -bp | awk '/^ *[0-9]+[mhd]/{print "exim -Mrm " $3}' | bash
```

---

## Quản lý Postfix (Chung)

### Cấu hình relay mail cho từng domain (Transport Maps)
Dùng để định tuyến mail của một số domain nhất định qua một smarthost/relay server khác.

**Yêu cầu:** Đăng nhập với user zimbra (nếu trên server Zimbra) hoặc root.

- Tạo hoặc sửa file transport:

    ```bash
    vi /opt/zimbra/postfix/conf/transport
    ```

    Nội dung file có dạng:

    ```
    # domain.com   relay:[ip.cua.relay.server]
    gmail.com      smtp:[103.104.123.15]
    hotmail.com    smtp:[103.104.123.15]
    ```

- Tạo file map cho Postfix:

    ```bash
    postmap /opt/zimbra/postfix/conf/transport
    ```

- Khai báo trong cấu hình Postfix:

    - Trên Zimbra, dùng lệnh:

        ```bash
        zmprov ms mail.yourserver.com zimbraMtaTransportMaps "lmdb:/opt/zimbra/postfix/conf/transport,proxy:ldap:/opt/zimbra/conf/ldap-transport.cf"
        ```

    - Trên server Postfix thường, sửa file main.cf:

        ```
        transport_maps = hash:/opt/zimbra/postfix/conf/transport
        ```

- Khởi động lại dịch vụ:

    ```bash
    # Trên Zimbra
    zmcontrol restart
    # Trên Postfix thường
    postfix reload
    ```

---

## Các cấu hình khác

### Lỗi 550 Sender verify failed

**Nguyên nhân:**  
Server đang cố gắng xác thực người gửi như một local user nhưng không tìm thấy.

**Giải pháp:**  
Chuyển domain của người gửi từ localdomains sang remotedomains.

- Xóa hoặc comment (#) domain bị lỗi trong file `/etc/localdomains`.
- Thêm domain đó vào file `/etc/remotedomains`.
- Restart Exim/Postfix.

---

### Khóa (Lock) port SMTP cho một domain trên Exim

Để tạm thời không cho một domain gửi mail đi.

- Sửa file `/etc/localdomains`.
- Thêm dấu `#` vào trước tên domain cần khóa.
- Restart Exim.

---

## Cấu hình iSend (Ví dụ cụ thể)

Đây là ví dụ về cấu hình Postfix để relay qua một dịch vụ SMTP (Amazon SES).

- SSH vào server iSend.
- Sửa file chứa thông tin đăng nhập:

    ```bash
    vi /etc/postfix/sasl_passwd
    ```

    Nội dung có dạng `[smtp.server.com]:port username:password`:

    ```
    email-smtp.us-east-1.amazonaws.com:25   AKIA...:AmVE...
    ```

- Tạo file map và reload:

    ```bash
    postmap hash:/etc/postfix/sasl_passwd
    postfix reload
    ```

- Cho phép IP gửi mail qua relay:

    - Sửa file `/etc/postfix/main.cf`.
    - Tìm dòng `mynetworks` và bổ sung IP được phép.
