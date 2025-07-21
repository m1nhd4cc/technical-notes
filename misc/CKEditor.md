## Xóa khoảng trắng trong tên file khi upload bằng CKFinder

**Mục đích:** Tự động thay thế khoảng trắng bằng dấu `_` để tránh lỗi URL.

**Bước 1: Bật chế độ chuyển ký tự không phải ASCII**
-   Mở file `ckfinder/config.php`.
-   Tìm và sửa dòng: `$config['ForceAscii'] = true;`

**Bước 2: Thêm quy tắc thay thế khoảng trắng**
-   Mở file:
    -   PHP 4: `ckfinder/core/connector/php/php4/Utils/FileSystem.php`
    -   PHP 5+: `ckfinder/core/connector/php/php5/Utils/FileSystem.php`
-   Tìm đến mảng `$_translit` (khoảng dòng 257).
-   Thêm một entry để thay thế khoảng trắng:
    ```php
    // Trong mảng chữ thường
    ' ' => '_',
    // Trong mảng chữ hoa
    ' ' => '_',
    ```

---

## Sửa lỗi nhập liệu trong CKEditor 4

**Vấn đề 1: CKEditor tự động xóa các thẻ HTML không mong muốn (ví dụ `<iframe>`).**
-   **Giải pháp:** Cho phép mọi loại nội dung.
-   Mở file `config.js` của CKEditor.
-   Thêm vào trong `CKEDITOR.editorConfig`:
    ```javascript
    config.allowedContent = true;
    ```

**Vấn đề 2: CKEditor tự động bọc thẻ `<a>` bằng `<figure>` hoặc `<div>`.**
-   **Giải pháp:** Sửa đổi định nghĩa DTD (Document Type Definition) của CKEditor.
-   Thêm dòng sau vào cuối file `config.js`:
    ```javascript
    CKEDITOR.dtd.a.figure = 1;
    CKEDITOR.dtd.a.div = 1;
    ```
