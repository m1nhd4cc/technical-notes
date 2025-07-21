<?php
// ===================================================================
// LẤY THÔNG TIN PHP
// ===================================================================

phpinfo();


// ===================================================================
// CHUỖI KẾT NỐI DATABASE (PHP)
// ===================================================================

$config['db']['host'] = 'localhost';
$config['db']['port'] = '3306';
$config['db']['username'] = 'mili_sa';
$config['db']['password'] = 'mjn@21@12@1990';
$config['db']['dbname'] = 'mili_milivn';


// ===================================================================
// CHUYỂN HƯỚNG (REDIRECT)
// ===================================================================

// Redirect tạm thời (302)
header('Location: http://www.new-website.com/');
exit;

// Redirect vĩnh viễn (301 - Tốt cho SEO)
header('Location: http://www.new-website.com/', true, 301);
exit();


// ===================================================================
// THAY ĐỔI CẤU HÌNH PHP RUNTIME
// ===================================================================

ini_set('display_errors', 'On');
ini_set('memory_limit', '512M');


// ===================================================================
// TEST KẾT NỐI MEMCACHED
// ===================================================================

$mem = new Memcached();
$mem->addServer("127.0.0.1", 11211);

$key = "my_test_key";
$result = $mem->get($key);

if ($result) {
    echo "Value found in cache: " . $result;
} else {
    echo "No matching key found. I'll add that now!";
    $mem->set($key, "I am data! I am held in memcached!", 3600) or die("Couldn't save anything to memcached...");
}


// ===================================================================
// CHẠY LỆNH BASH SHELL TỪ PHP (CẨN TRỌNG BẢO MẬT)
// ===================================================================
/*
 * Phương pháp này cho phép file PHP thực thi một file .sh với quyền root.
 * CỰC KỲ NGUY HIỂM - Chỉ sử dụng khi thực sự cần thiết và hiểu rõ rủi ro.
 */

// 1. Tạo file .sh (ví dụ: /path/to/script.sh) và phân quyền chặt chẽ.
//    #!/bin/bash
//    echo "Hello from Shell" > /tmp/shell_output.txt

// 2. Tạo file C (wrapper.c) để gọi file shell với quyền root.
/*
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
int main (int argc, char *argv[]) {
   setuid (0);
   system ("/usr/bin/sh /path/to/script.sh");
   return 0;
}
*/

// 3. Biên dịch file C và set SUID bit.
//    gcc wrapper.c -o php_root
//    chown root:root php_root
//    chmod u=rwx,go=xr,+s php_root

// 4. Gọi file thực thi từ PHP (cần mở `shell_exec` cho user này).
$output = shell_exec('/path/to/php_root 2>&1');
echo "<pre>$output</pre>";


// ===================================================================
// TRUYỀN BIẾN TỪ PHP SANG BASH SHELL
// ===================================================================

// File PHP
putenv("MY_VARIABLE=HelloFromPHP");
$output = exec('./my_script.sh');
echo $output;

// File my_script.sh
// #!/bin/bash
// echo "The variable from PHP is: $MY_VARIABLE"

?>

