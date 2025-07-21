## Chỉnh thông số memory limit (watermark) cho RabbitMQ

**Mục đích:** Ngăn RabbitMQ sử dụng quá nhiều RAM, gây treo hệ thống. Watermark là ngưỡng RAM mà khi đạt đến, RabbitMQ sẽ bắt đầu block các publisher.

**Bước 1: Tạo file cấu hình môi trường**
-   Tạo và sửa file: `/etc/rabbitmq/rabbitmq-env.conf`
-   Thêm vào nội dung sau để chỉ định đường dẫn file config chính:
    ```
    RABBITMQ_CONFIG_FILE=/etc/rabbitmq/rabbitmq.conf
    ```

**Bước 2: Tạo file cấu hình chính**
-   Tạo và sửa file: `/etc/rabbitmq/rabbitmq.conf`
-   Thêm vào nội dung sau để đặt watermark ở mức 90% tổng RAM:
    ```ini
    # Set the high-memory watermark to 90% of total RAM.
    vm_memory_high_watermark.relative = 0.9
    ```
    > **Lưu ý:** Bạn cũng có thể đặt một giá trị tuyệt đối, ví dụ: `vm_memory_high_watermark.absolute = 2GB`.

**Bước 3: Áp dụng thay đổi (Cách 1 - Khởi động lại)**
```bash
systemctl restart rabbitmq-server
```

**Bước 4: Áp dụng thay đổi (Cách 2 - Runtime, không cần restart)**
```bash
rabbitmqctl set_vm_memory_high_watermark 0.9
```
Lệnh này sẽ thay đổi watermark ngay lập tức nhưng sẽ mất khi restart nếu không được cấu hình trong file `.conf`