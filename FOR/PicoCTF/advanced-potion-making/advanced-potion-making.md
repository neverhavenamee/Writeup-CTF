# advanced-potion-making

Source: PicoCTF
Tools: HexEditor, fotoforensics
Technique: file signature
Fields: forensic

- the file ‘s stucture changed
- file PNG( hallmark: IHDR)
- Change `89 50 42 11 0D 0A 1A 0A 00 12 13 14 49` to `89 50 4E 47 0D 0A 1A 0A 00 00 00 0D` and save as file.png = hexeditor
- → fotoforensics
- picoCTF{w1z4rdry}