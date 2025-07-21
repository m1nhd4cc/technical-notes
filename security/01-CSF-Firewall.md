# CÁC LỆNH CƠ BẢN CỦA CSF (CONFIGSERVER SECURITY & FIREWALL)

# Kích hoạt firewall
csf -e

# Vô hiệu hóa firewall (tạm thời)
csf -x

# Khởi động lại firewall (áp dụng thay đổi)
csf -r

# QUẢN LÝ IP (ALLOW / DENY)

# Cho phép (whitelist) một IP
csf -a 123.123.123.123 "Ghi chu cho IP nay"

# Xóa một IP khỏi danh sách cho phép
csf -ar 123.123.123.123

# Chặn (blacklist) một IP
csf -d 123.123.123.123 "Ly do chan"

# Xóa một IP khỏi danh sách chặn
csf -dr 123.123.123.123

# CẤU HÌNH NÂNG CAO

### Mở hoặc đóng Port
# Sửa file /etc/csf/csf.conf
# Tìm các dòng TCP_IN, TCP_OUT, UDP_IN, UDP_OUT và thêm/xóa port.
# Ví dụ:
# TCP_IN = "20,21,22,25,53,80,110,143,443,465,587,993,995"

### Chặn truy cập theo quốc gia
# 1. Sửa file /etc/csf/csf.conf
# 2. Tìm đến mục CC_DENY và CC_ALLOW
# 3. Thêm mã quốc gia (ISO 3166-1 alpha-2) vào danh sách, cách nhau bởi dấu phẩy.
#    Ví dụ, chặn truy cập từ Trung Quốc và Nga:
#    CC_DENY = "CN,RU"
# 4. Khởi động lại CSF: csf -r

### Block port LDAP (389, 636)
# 1. Sửa file /etc/csf/csf.deny
# 2. Thêm vào cuối file:
#    udp|in|d=389
#    udp|out|d=389
#    tcp|in|d=389
#    tcp|out|d=389
#    udp|in|d=636
#    udp|out|d=636
#    tcp|in|d=636
#    tcp|out|d=636
# 3. Khởi động lại CSF: csf -r

### Block toàn bộ kết nối vào port 80 (Dùng khi bị DDOS nặng)
# CẢNH BÁO: Lệnh này sẽ làm website không thể truy cập.
#
# 1. Allow IP của bạn trước để không bị khóa:
#    csf -a YOUR_IP_ADDRESS
# 2. Block toàn bộ kết nối vào port 80 trong 300 giây:
#    csf -td 0.0.0.0/0 300 -p 80 -d in "Block all port 80 for 5 mins"
# 3. Block vĩnh viễn (cho đến khi gỡ bằng tay):
#    csf -d 0.0.0.0/0 -p 80 -d in "Block all port 80"

