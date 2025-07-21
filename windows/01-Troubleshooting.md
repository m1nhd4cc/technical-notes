## Không kết nối được FTP trên Windows Server (Plesk)

**Lỗi:** `534 Protection level negotiation failed.`

**Nguyên nhân:**
Server Plesk được cấu hình để yêu cầu kết nối FTPS (FTP bảo mật), nhưng client đang cố gắng kết nối bằng FTP thường.

**Giải pháp (trên Plesk Panel):**
1.  Đăng nhập Plesk với tài khoản `admin`.
2.  Đi đến `Tools & Settings` -> `IP Addresses`.
3.  Click vào địa chỉ IP của server.
4.  Check vào ô `Allow both secure FTPS and non-secure FTP connections`.
5.  Lưu lại.

---

## Sửa USB bị lỗi phân vùng hoặc không thể format

**Cảnh báo:** Các lệnh sau sẽ xóa toàn bộ dữ liệu trên USB.

**Sử dụng `diskpart`:**
1.  Mở Command Prompt với quyền Administrator.
2.  Gõ `diskpart` và nhấn Enter.
3.  Thực hiện tuần tự các lệnh sau trong `diskpart`:
    ```dos
    list disk                  # Liệt kê các ổ đĩa, xác định số của USB
    select disk X              # Thay X bằng số của USB
    clean                      # Xóa toàn bộ thông tin phân vùng
    create partition primary   # Tạo lại phân vùng chính
    select partition 1         # Chọn phân vùng vừa tạo
    active                     # Kích hoạt phân vùng
    format fs=ntfs quick       # Format nhanh với định dạng NTFS
    assign                     # Tự động gán một ký tự cho ổ đĩa
    exit                       # Thoát diskpart
    ```

---

## Xóa file/thư mục có tên hoặc đường dẫn quá dài

**Sử dụng `robocopy`:**
1.  Mở Command Prompt.
2.  `cd` đến thư mục chứa file/thư mục cần xóa.
3.  Tạo một thư mục rỗng tạm thời:
    ```dos
    mkdir empty_dir
    ```
4.  Dùng `robocopy` để "mirror" thư mục rỗng vào thư mục cần xóa. Thao tác này sẽ xóa sạch nội dung bên trong.
    ```dos
    robocopy empty_dir "ten_thu_muc_can_xoa" /s /mir
    ```
5.  Xóa thư mục (giờ đã rỗng) và thư mục tạm:
    ```dos
    rmdir "ten_thu_muc_can_xoa"
    rmdir empty_dir
    ```

---

## Script tự động cập nhật DNS cho card mạng

Lưu nội dung dưới đây thành file `.bat`. Khi chạy, nó sẽ đặt DNS chính và phụ cho tất cả các card mạng đang được kích hoạt.
```batch
@echo off
setlocal

set DNSSERVER1=8.8.8.8
set DNSSERVER2=8.8.4.4

echo Changing DNS servers to %DNSSERVER1% and %DNSSERVER2%...

for /f "tokens=1,2,3*" %%i in ('netsh interface show interface') do (
    if %%i EQU Enabled (
        echo Updating "%%l"...
        netsh interface ipv4 set dnsserver name="%%l" static %DNSSERVER1% primary
        netsh interface ipv4 add dnsserver name="%%l" %DNSSERVER2% index=2
    )
)

echo Done.
endlocal
pause
```

---

## Lỗi Active Directory / Group Policy không vào được

**Nguyên nhân & Giải pháp:**

- **Sai cấu hình DNS:** Đây là nguyên nhân hàng đầu.
    - Vào cấu hình card mạng của Domain Controller -> IPv4.
    - Trong mục DNS, chỉ được trỏ về chính nó (ví dụ: 127.0.0.1 hoặc IP nội bộ của DC). Tuyệt đối không trỏ ra DNS ngoài (Google, Cloudflare,...).

- **Service không chạy:**
    - Mở `services.msc`.
    - Kiểm tra và đảm bảo các service sau đang chạy và được đặt chế độ Automatic:
        - NetLogon
        - DFS Namespace
        - DNS Server
        - Active Directory Domain Services

---

## Không đổi được mật khẩu trên Domain Controller

**Nguyên nhân:**
Mặc dù đã tắt "Password complexity" trong Group Policy, nhưng chính sách bảo mật cục bộ (Local Security Policy) vẫn được áp dụng và có độ ưu tiên cao hơn.

**Giải pháp:**

- Mở Local Security Policy (`secpol.msc`).
- Đi đến `Account Policies` -> `Password Policy`.
- Chỉnh lại các thiết lập trong này (độ dài tối thiểu, độ phức tạp,...) cho khớp với mong muốn.