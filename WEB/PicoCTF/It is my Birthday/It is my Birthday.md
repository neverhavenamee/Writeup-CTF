# It is my Birthday

Source: PicoCTF

Fields: Web

I searched up "MD5 collision" and eventually found [this](https://www.mscs.dal.ca/~selinger/md5collision/) website. It provided 2 executable files ([hello](https://vivian-dai.github.io/PicoCTF2021-Writeup/Web%20Exploitation/It%20is%20my%20Birthday/hello.pdf) and [erase](https://vivian-dai.github.io/PicoCTF2021-Writeup/Web%20Exploitation/It%20is%20my%20Birthday/erase.pdf)) which have the same MD5 hash. I downloaded those files and changed the extension to a .pdf file.

I uploaded those two files and the website redirected to the [PHP](https://vivian-dai.github.io/PicoCTF2021-Writeup/Web%20Exploitation/It%20is%20my%20Birthday/source.php):

The flag can be found in a comment at the end of the PHP (before the HTML portion, line [37](https://github.com/vivian-dai/PicoCTF2021-Writeup/blob/53bed8a7177440cca590ed45ed4a6ed142ca8515/Web%20Exploitation/It%20is%20my%20Birthday/source.php#L37))
