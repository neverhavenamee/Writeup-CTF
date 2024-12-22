# OS command injection, simple case

Source: Portswigger
Tools: Burpsuite
Technique: OS Command Injection
Fields: Web

- Modify the `storeID` parameter, giving it the value `1|whoami`.