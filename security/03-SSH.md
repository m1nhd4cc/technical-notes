# ===================================================================
# QUẢN LÝ KEY SSH
# ===================================================================

### Tạo cặp key SSH mới trên server
# Lệnh này sẽ tạo ra 2 file trong ~/.ssh/: id_rsa (private key) và id_rsa.pub (public key)
ssh-keygen -t rsa -b 4096

### Xem public key để copy
cat ~/.ssh/id_rsa.pub

### Thêm public key của máy client vào server để cho phép đăng nhập không cần mật khẩu
# Copy nội dung public key của client và dán vào file sau trên server:
vi /root/.ssh/authorized_keys

# Phân quyền đúng cho thư mục .ssh và file authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# ===================================================================
# BẢO MẬT SSH
# ===================================================================

### Chỉ cho phép một số IP nhất định được SSH
# Cách này sử dụng TCP Wrappers, an toàn hơn việc chỉ dựa vào firewall.

# 1. Chặn tất cả kết nối SSH mặc định
#    Sửa file /etc/hosts.deny và thêm vào cuối:
#    sshd: ALL

# 2. Cho phép các IP được tin tưởng
#    Sửa file /etc/hosts.allow và thêm vào cuối:
#    sshd: 115.79.62.130, 125.212.244.247, 222.255.236.80

# Lưu ý: Luôn đảm bảo IP của bạn nằm trong danh sách allow trước khi thực hiện.
# Kết hợp với việc allow port 22 trên CSF cho các IP này.

### Xem log đăng nhập SSH
# Xem các lần đăng nhập thất bại
grep 'sshd.*Invalid' /var/log/secure
grep 'sshd.*Failed' /var/log/secure

# Xem các lần đăng nhập thành công
grep 'sshd.*Accepted' /var/log/secure
grep 'sshd.*session.*opened' /var/log/secure

