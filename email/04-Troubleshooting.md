---
---

## File: `/04-Email/04-Troubleshooting.md`

## Nhận email bị trùng lặp (duplicate) trên Outlook

**Nguyên nhân:**
- Cấu hình "Leave a copy of messages on the server" được bật nhưng không có thời gian xóa.
- Profile Outlook bị lỗi.
- Antivirus quét email đến.

**Tham khảo cách khắc phục:**
- [Science Opposing Views](http://science.opposingviews.com/stop-microsoft-outlook-receiving-duplicate-emails-8507.html)
- [Microsoft Support](https://support.microsoft.com/en-us/kb/2685726)

---

## Nhận được file `winmail.dat` thay vì file đính kèm

**Nguyên nhân:**
Người gửi sử dụng Outlook với định dạng Rich Text (RTF). Khi email này đi qua các mail client không phải của Microsoft, nó sẽ bị chuyển thành file `winmail.dat`.

**Giải pháp:**
- Yêu cầu người gửi chuyển định dạng soạn thảo email của họ sang HTML hoặc Plain Text.
- **Tham khảo hướng dẫn của Microsoft:** [How to prevent Winmail.dat attachments from being sent in Outlook](https://support.microsoft.com/en-us/kb/278061)

---

## Chữ ký email không giữ được định dạng trên RoundCube

**Nguyên nhân:**
Chế độ soạn thảo email đang được đặt là `Plain Text` thay vì `HTML`.

**Giải pháp:**
1. Đăng nhập vào webmail RoundCube.
2. Vào `Settings` -> `Preferences` -> `Composing Messages`.
3. Trong mục `Compose HTML messages`, chọn `Always`.
4. Nhấn `Save`.

---

## Google Mail từ chối truy cập SMTP (không gửi được mail contact từ website)

**Nguyên nhân:**
Google coi ứng dụng (website của bạn) là "kém an toàn" và chặn truy cập để bảo vệ tài khoản.

**Giải pháp (Thực hiện tuần tự):**

1. **Thử gửi mail từ website 1 lần:** Việc này sẽ thất bại nhưng sẽ tạo ra một log truy cập bị chặn trong tài khoản Google.
2. **Đăng nhập vào tài khoản Gmail** được cấu hình trên website bằng trình duyệt.
3. **Bật quyền truy cập cho ứng dụng kém an toàn:**
    - Truy cập: [https://www.google.com/settings/security/lesssecureapps](https://www.google.com/settings/security/lesssecureapps)
    - Bật (Turn on) tùy chọn này.
4. **Mở khóa Captcha:**
    - Truy cập: [https://accounts.google.com/DisplayUnlockCaptcha](https://accounts.google.com/DisplayUnlockCaptcha)
    - Nhấn "Continue" để cho phép truy cập.
5. **Xác nhận hoạt động đăng nhập:**
    - Vào trang quản lý tài khoản Google (`myaccount.google.com`).
    - Chọn `Security` (Bảo mật).
    - Tìm đến mục `Recent security activity` (Hoạt động bảo mật gần đây) và tìm sự kiện "Sign-in attempt was blocked".
    - Click vào sự kiện đó và chọn "Yes, it was me" (Vâng, đó là tôi).
6. **Thử gửi lại email từ website.**

