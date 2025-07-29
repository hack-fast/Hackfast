---
legal-banner: true
---

2. Response manipulation.
3. Use previously used captcha value.
4. Use any token with same length(+1/-1).
5. Remove the param value or remove the entire parameter.
6. Change method from POST to GET(or PUT) and remove the captcha.
7. Change body to JSON or vice-versa.
8. Try OCR.
9. Check whether the captcha is in the source code. (Ex: 2+2)
10. Check whether the value of the captcha is in the source code.
11. Try to use the same captcha value several times with the different sessionIDs.
12. JSON Body? Combine 6 and 5.
Add headers:
```
X-Forwarded-Host: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Originating-IP: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Client-IP: 127.0.0.1
X-Host: 127.0.0.1
```

Todo