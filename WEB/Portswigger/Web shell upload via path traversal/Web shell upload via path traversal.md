# Web shell upload via path traversal

Source: Portswigger
Tools: Burpsuite
Technique: FIle upload, Path Traversal
Fields: Web

upload php file name: ../exploit.php để thoát ra ngoài thư mục cấm execute php

- Note: khi sử dụng burpsuite → phải encode : ../exploit.php → ..%2fexploit.php