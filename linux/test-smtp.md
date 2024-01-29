# Test SMTP server using telnet

There is a list of commands that helps to test SMTP server

```shell
telnet smtp.example.com 587
```

```shell
EHLO yourdomain.com
STARTTLS (if using a secure connection)
MAIL FROM: your_email@example.com
RCPT TO: recipient_email@example.com
DATA
Subject: Your Subject
Your email content goes here.
.
QUIT

```