# Web shell upload via extension blacklist bypass

Source: Portswigger
Tools: Burpsuite
Technique: FIle upload
Fields: Web

- upload .htaccess file to set configuration allow web server to execute file with arbitrary extension

```java
AddType application/x-httpd-php .hieu (extension)
```

- Then upload file php with .hieu extension