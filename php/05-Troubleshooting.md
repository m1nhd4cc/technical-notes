## Lỗi: Fatal error: Incompatible file format

**Thông báo lỗi:**
> Fatal error: Incompatible file format: The encoded file has format major ID 3, whereas the Loader expects 4 in /home/mocly/public_html/config.php on line 0

**Nguyên nhân:**
File PHP đã được mã hóa bằng một phiên bản Zend Guard/Optimizer cũ hơn so với phiên bản Zend Loader đang được cài trên server.

**Giải pháp:**
-   Hạ cấp phiên bản PHP của website xuống một phiên bản cũ hơn có hỗ trợ Zend Optimizer tương thích (ví dụ: PHP 5.2).
-   Yêu cầu nhà cung cấp mã nguồn cung cấp file đã được mã hóa bằng phiên bản mới hơn.

## Lỗi khi compile PHP cũ với libxml2 > 2.9.0

**Thông báo lỗi:**
> make: *** [ext/dom/node.lo] Error 1

**Nguyên nhân:**
Các phiên bản PHP cũ (ví dụ 5.x) không tương thích với những thay đổi trong thư viện `libxml2` từ phiên bản 2.9.1 trở đi.

**Giải pháp:**
Áp dụng một bản vá (patch) trước khi biên dịch.

1.  Tải file vá `libxml29_compat.patch` vào thư mục source của PHP.
2.  Chạy lệnh patch:
    ```bash
    patch -p1 < libxml29_compat.patch
    ```
3.  Tiến hành `make` và `make install` lại.

