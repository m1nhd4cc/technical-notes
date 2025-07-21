## Quản lý DKIM trên Zimbra

**Yêu cầu:** Đăng nhập với user `zimbra`.

```bash
su - zimbra

# Tạo DKIM cho một domain:
/opt/zimbra/libexec/zmdkimkeyutil -a -d yourdomain.com

# Lấy thông tin record DKIM để cấu hình DNS:
/opt/zimbra/libexec/zmdkimkeyutil -q -d yourdomain.com

# Cập nhật DKIM (sau khi đã tạo):
/opt/zimbra/libexec/zmdkimkeyutil -u -d yourdomain.com

# Xóa DKIM của một domain:
/opt/zimbra/libexec/zmdkimkeyutil -r -d yourdomain.com
```

---

## Quản lý hàng đợi mail (Mail Queue)

**Yêu cầu:** Cần quyền root hoặc zimbra tùy lệnh.

- Xem tổng quan hàng đợi (với user zimbra):

    ```bash
    /opt/zimbra/libexec/zmqstat
    # Output: hold=0, corrupt=0, deferred=0, active=0, incoming=0
    ```

- Liệt kê chi tiết các mail trong hàng đợi (với user zimbra):

    ```bash
    /opt/zimbra/common/sbin/postqueue -p
    ```

- Xử lý hàng đợi (với user root):

    ```bash
    # Xóa một mail cụ thể khỏi hàng đợi
    /opt/zimbra/common/sbin/postsuper -d [MSGID]

    # Xóa TẤT CẢ mail trong hàng đợi (CẨN THẬN!)
    /opt/zimbra/common/sbin/postsuper -d ALL

    # Xóa tất cả mail trong hàng đợi `deferred`
    /opt/zimbra/common/sbin/postsuper -d ALL deferred

    # Xóa tất cả mail trong hàng đợi `hold`
    /opt/zimbra/common/sbin/postsuper -d ALL hold

    # Đẩy lại toàn bộ hàng đợi
    /opt/zimbra/common/sbin/postqueue -f

    # Đưa một mail vào hàng đợi `hold`
    /opt/zimbra/common/sbin/postsuper -h [MSGID]

    # Thả một mail ra khỏi hàng đợi `hold`
    /opt/zimbra/common/sbin/postsuper -H [MSGID]
    ```

---

## Backup và Restore Mailbox

**Yêu cầu:** Đăng nhập với user zimbra.

- Backup một mailbox:

    ```bash
    /opt/zimbra/bin/zmmailbox -z -m user@yourdomain.com getRestURL "//?fmt=tgz" > /path/to/backup/user.tgz
    ```

- Restore một mailbox:

    ```bash
    /opt/zimbra/bin/zmmailbox -z -m user@yourdomain.com postRestURL "//?fmt=tgz&resolve=skip" /path/to/backup/user.tgz
    ```

- Các tùy chọn cho resolve khi restore:
    - `skip`: (Mặc định & An toàn nhất) Chỉ khôi phục những mail không có trong hộp thư. Bỏ qua các mail đã tồn tại.
    - `modify`: Khôi phục mail bị xóa và cập nhật trạng thái (đã đọc/chưa đọc) của các mail đang có cho khớp với bản backup.
    - `reset`: Xóa sạch mailbox hiện tại trước khi khôi phục.
    - `replace`: Khôi phục lại, ghi đè các mục hiện có nếu chúng tồn tại trong bản backup.

---

## Các lệnh quản trị khác

- Lấy mật khẩu MySQL của Zimbra:

    ```bash
    su - zimbra
    zmlocalconfig -s | grep mysql | grep password
    ```

- Đổi port quản lý từ xa (Remote Management Port):

    ```bash
    su - zimbra
    zmprov ms mail.yourserver.com zimbraRemoteManagementPort 22468
    ```

- Export danh sách account:

    ```bash
    su - zimbra
    # Toàn bộ account trên server
    zmprov -l gaa

    # Toàn bộ account của một domain
    zmprov -l gaa yourdomain.com
    ```

---

## Chặn địa chỉ mail (Blacklist)

1. Thêm cấu hình vào `smtpd_sender_restrictions.cf`  
   Thêm `check_sender_access hash:/opt/zimbra/postfix/conf/postfix_reject_sender` vào đầu file `/opt/zimbra/conf/zmconfigd/smtpd_sender_restrictions.cf`
2. Tạo file blacklist  
   `vi /opt/zimbra/postfix/conf/postfix_reject_sender`
3. Thêm email cần chặn với cú pháp:  
   `user@spamdomain.com   REJECT`
4. Chạy các lệnh sau với user `zimbra`:

    ```bash
    postmap /opt/zimbra/postfix/conf/postfix_reject_sender
    postfix reload
    ```

---

## Tắt tính năng chặn file mã hóa của Antivirus

```bash
su - zimbra
# Kiểm tra trạng thái
zmprov gacf | grep -i zimbraVirusBlockEncryptedArchive
# Tắt
zmprov mcf zimbraVirusBlockEncryptedArchive FALSE
```

---

## Bỏ tag UNCHECKED khi antivirus lỗi

- Sửa file `/opt/zimbra/amavisd/sbin/amavisd`
- Tìm dòng: `$undecipherable_subject_tag =`
- Sửa thành:  
  `$undecipherable_subject_tag = '';`
