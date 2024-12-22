# Whitepage

Source: PicoCTF

Tools: Code

Technique: whitespace character

Fields: forensic

- Open downloaded file, we received an empty file  —> let’s try Ctrl + A

![Untitled](Untitled.png)

- The name of the challenge make me think about [whitespace characters](https://en.wikipedia.org/wiki/Whitespace_character) Those characters are invisible but still are in the file. So after some searching in the web I saw a interpreter for those white-spaces, but nothing came out. So I go back to the file to check if all the chars are the same or there are different white-spaces in the file. I copy the first char and Ctrl+F in the file -
- As you can see, there some holes so this is different white-spaces chars, I copy one of the “holes” and search too, I found that the file include only 2 diffrenet white-spaces. So after some time I thought about binary, maybe those chars signal 1 and 0s.
- So i use this python script to change the text into 1 0

```python
text = '                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                '

firstType = ' '
secondType =  ' '
binaryString = ''

for char in text: 
	if char == firstType: 
		binaryString += '0' 
	else:
		binaryString += '1' 

print(binaryString)
```

```python
00001010000010010000100101110000011010010110001101101111010000110101010001000110000010100000101000001001000010010101001101000101010001010010000001010000010101010100001001001100010010010100001100100000010100100100010101000011010011110101001001000100010100110010000000100110001000000100001001000001010000110100101101000111010100100100111101010101010011100100010000100000010100100100010101010000010011110101001001010100000010100000100100001001001101010011000000110000001100000010000001000110011011110111001001100010011001010111001100100000010000010111011001100101001011000010000001010000011010010111010001110100011100110110001001110101011100100110011101101000001011000010000001010000010000010010000000110001001101010011001000110001001100110000101000001001000010010111000001101001011000110110111101000011010101000100011001111011011011100110111101110100010111110110000101101100011011000101111101110011011100000110000101100011011001010111001101011111011000010111001001100101010111110110001101110010011001010110000101110100011001010110010001011111011001010111000101110101011000010110110001011111011000110011010100110100011001100011001000110111011000110110010000110000001101010110001100110010001100010011100000111001011001100011100000110001001101000011011101100011011000110011011001100110001101010110010001100101011000100011001001100101001101010011011001111101000010100000100100001001
```

- decode with binary type, we’ll get flag

```python
picoCTF

		SEE PUBLIC RECORDS & BACKGROUND REPORT
		5000 Forbes Ave, Pittsburgh, PA 15213
		picoCTF{not_all_spaces_are_created_equal_c54f27cd05c2189f8147cc6f5deb2e56}
```
