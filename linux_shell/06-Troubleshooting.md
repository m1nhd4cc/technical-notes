---
---

## File: `/06-Linux-Shell/06-Troubleshooting.md`

## Lỗi: setlocale: LC_CTYPE: cannot change locale (UTF-8)

**Nguyên nhân:**  
Hệ thống thiếu cấu hình locale UTF-8 cần thiết.

**Giải pháp:**  
Cài đặt lại cấu hình locale và định nghĩa các biến môi trường.

1. **Sửa file `/etc/environment`:**
    ```bash
    vi /etc/environment
    ```
    Thêm vào các dòng sau:
    ```
    LANG=en_US.utf-8
    LC_ALL=en_US.utf-8
    ```
2. **Cài đặt lại `glibc-common`:**
    ```bash
    yum reinstall glibc-common -y
    ```
3. **Đăng xuất và đăng nhập lại** để áp dụng thay đổi.

---

## Lỗi: Permission denied khi chạy script .sh

**Nguyên nhân:**  
File script không có quyền thực thi.

**Giải pháp:**  
Cấp quyền thực thi cho file script:

```bash
chmod +x script.sh
```

---

## Lỗi: command not found

**Nguyên nhân:**  
- Gõ sai tên lệnh.
- Lệnh không có trong biến môi trường `$PATH`.
- Chưa cài đặt gói phần mềm chứa lệnh đó.

**Giải pháp:**  
- Kiểm tra lại tên lệnh.
- Kiểm tra biến `$PATH`:
    ```bash
    echo $PATH
    ```
- Cài đặt gói phần mềm nếu thiếu, ví dụ:
    ```bash
    yum install <package

