# SOAP

Source: PicoCTF

Tools: Burpsuite

Technique: XXE vul

Fields: Web

![Untitled](Untitled.png)

- Sử dụng proxy burpsuite để bắt package

![Untitled](Untitled%201.png)

- Khi click vào button Details ta bắt đc một package có body là một đoạn mã xml cơ bản dễ dàng exploit bằng cách thay đổi đoạn mã xml

![Untitled](Untitled%202.png)

![Untitled](Untitled%203.png)

- Sau khi gửi đến repeater để send request, ta nhận được response sau

![Untitled](Untitled%204.png)
