## Thêm ổ đĩa mới và Mount vào hệ thống (CentOS)

**Kịch bản:** Gắn thêm một ổ đĩa vật lý hoặc ảo (`/dev/sdb`) và mount vào thư mục `/backup`.

### Bước 1: Tạo Partition
```bash
# Xem danh sách các ổ đĩa
fdisk -l

# Bắt đầu tạo partition cho ổ đĩa /dev/sdb
fdisk /dev/sdb
```
Trong giao diện fdisk:
- Gõ `n` để tạo partition mới.
- Gõ `p` để chọn primary partition.
- Gõ `1` để tạo partition đầu tiên (`/dev/sdb1`).
- Nhấn `Enter` 2 lần để chấp nhận sector đầu và cuối mặc định (dùng toàn bộ ổ đĩa).
- Gõ `w` để ghi thay đổi và thoát.

---

### Bước 2: Định dạng Partition

```bash
# Định dạng partition mới với hệ thống file XFS (phổ biến trên CentOS 7+)
mkfs.xfs /dev/sdb1
```

---

### Bước 3: Mount Partition

```bash
# Tạo thư mục để mount
mkdir /backup

# Mount partition vào thư mục
mount /dev/sdb1 /backup
```

---

### Bước 4: Tự động Mount khi khởi động

Để ổ đĩa không bị mất mount sau khi reboot, cần thêm vào file `/etc/fstab`.

Lấy UUID của partition để mount ổn định hơn:

```bash
blkid /dev/sdb1
# Output ví dụ: /dev/sdb1: UUID="a1b2c3d4-..." TYPE="xfs"
```

Sửa file `/etc/fstab`:

```bash
vi /etc/fstab
```

Thêm dòng sau vào cuối file (thay UUID bằng giá trị bạn lấy được):

```
UUID="a1b2c3d4-..."   /backup   xfs   defaults   0 0
```

---

### Bước 5: Kiểm tra

```bash
# Mount tất cả các entry trong fstab để kiểm tra lỗi
mount -a

# Kiểm tra lại bằng lệnh df
df -h
```

---

## Quản lý LVM (Logical Volume Management)

LVM cho phép quản lý không gian đĩa linh hoạt hơn, dễ dàng thay đổi kích thước phân vùng.

**Tham khảo hướng dẫn chi tiết:**
