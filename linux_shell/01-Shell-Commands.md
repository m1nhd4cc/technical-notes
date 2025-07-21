# ===================================================================
# XEM THÔNG TIN HỆ THỐNG
# ===================================================================

# Xem thông tin CPU
cat /proc/cpuinfo

# Xem thông tin RAM
cat /proc/meminfo
free -m

# Xem phiên bản Kernel
cat /proc/version

# Xem phiên bản CentOS/RHEL
cat /etc/redhat-release

# Xem phiên bản PHP
php -v

# ===================================================================
# QUẢN LÝ FILE VÀ THƯ MỤC
# ===================================================================

# Xem dung lượng ổ cứng
df -h

# Xem dung lượng của một thư mục
du -sh /path/to/directory

# Xem dung lượng của các file/thư mục con bên trong
du -sh *

# Liệt kê 10 file/thư mục lớn nhất trong thư mục hiện hành
du -hs * | sort -rh | head -10

# Tìm và xóa các file rỗng (0 byte)
find . -type f -size 0 -delete

# Tìm và xóa các thư mục cũ hơn 14 ngày
find /path/to/backup/* -type d -mtime +14 -exec rm -rf {} \;

# Tìm một chuỗi trong tất cả các file
grep -rnw '/path/to/search/' -e 'pattern'

# Tìm một file theo tên
find / -name "filename.txt"

# ===================================================================
# QUẢN LÝ TIẾN TRÌNH (PROCESS)
# ===================================================================

# Kiểm tra server có bị DDOS hay không
# Liệt kê các IP đang kết nối đến port 80 và số lượng kết nối
netstat -an | grep ':80' | awk '{print $5}' | cut -d':' -f1 | sort | uniq -c | sort -nr | head -20

# Xem các loại kết nối đến port 80
netstat -an | grep :80 | awk '{print $6}' | sort | uniq -c

# Xem tổng số kết nối đến port 80
netstat -n | grep :80 | wc -l

# ===================================================================
# LÀM VIỆC VỚI FILE TEXT
# ===================================================================

# Xóa dòng 1 và 2 trong file
sed -i '1d;2d' tenfile.txt

# Xóa từ dòng 5 đến dòng 10
sed -i '5,10d' tenfile.txt

# Xóa tất cả các dòng KHÔNG chứa từ khóa "baohaiquan"
sed -i '/baohaiquan/!d' tenfile.txt

# ===================================================================
# MẠNG VÀ KẾT NỐI
# ===================================================================

# Đổi hostname (CentOS 7) không cần reboot
nmcli general hostname new-hostname.domain.com
systemctl restart systemd-hostnamed

# Giải phóng bộ nhớ cache (RAM)
# Chuyển dữ liệu từ buffer vào disk và xóa cache
sync; echo 3 > /proc/sys/vm/drop_caches

# ===================================================================
# SCP (Secure Copy)
# ===================================================================

# Copy thư mục từ local sang remote server
scp -r /path/from/local username@hostname:/path/to/remote

# Copy thư mục từ remote server về local
scp -r username@hostname:/path/from/remote /path/to/local

# Copy với port SSH tùy chỉnh
scp -r -P 2234 /path/from/local username@hostname:/path/to/remote

# ===================================================================
# RSYNC
# ===================================================================

# Đồng bộ hóa thư mục, có nén, xóa file gốc sau khi xong, và dùng port tùy chỉnh
rsync -azv -e 'ssh -p 2234' --remove-source-files /path/to/source/ user.* root@remote.server:/path/to/dest/

