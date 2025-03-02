# Shadow Auth

---

### Challenge

We have the website [ShadowAuth](https://thiranctf-shadow-auth.chals.io/) and we need to uncover the hidden flag from it.

---

### Solution

On the homepage of the website, we see a page that says 'Public Key', and upon visiting it, we find the public key printed in plain text format. Let's keep it aside for now and inspect the webpage further.

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA1OXYfQtDLU2HYvhpip9w
scc/9vubuVbnuD3B1VnNLjQhI0dU1VBWhwc6JQjrceCjFh6y+S22JKjGNeQwjxZM
3jgGeOGl9unhBcSKna4n1yWQRsca60RfGbpeDxBYLRjSCLy8y7BafRh7Rqie0MaF
Q+XapY9oPSUDeJIEH2IG1iHbTcjUzevtphmfdDnOIW27cm0s7T+WyiPQeRx8vIYx
5cU3wFpJIEnQ47LTcqRgDcTTeoPp0bQDUwFEqswmMcsQGCkzL2vu+iOBTzsVbQgA
uhGZx6zZ4LyMIpOE0q//D5Ou5gSp3MK+Rr9LoraLXj8KCMf/xuXxzp1pNCbmpmmi
rQIDAQAB
-----END PUBLIC KEY-----
```

Since there is an Admin page, we try to access it directly... but find that we're redirected to the login page. Try giving random credentials to see what happens.

![login page](https://github.com/user-attachments/assets/bd54977a-addc-4a2c-9673-a16152b20ce6)

```
Username: test
Password: test
```

This generates the following cookie:

```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3QiLCJyb2xlIjoidXNlciJ9.
F-H4EW7TsCvr1Z0kkN1scZZKIuhJdu87psN9ZR1W4VfUboPL3qqcqm8kMJ5dwioFaZLdaNsQnbTF79RVet
dnKy8V-02Nqfir3Ru-sgTWA-_Etpjqo5ne0zNg6FDIihXS_cfNw9z7i03Ogo4OPedsgbOBqrxMgar7KkZX
DF2LDTgLzVvUlDKTRyWSlpjh6uln9oKuvxe5iZCOYfCtACJklvy7868VPopEweLZ5bG_ljsHj10q1TAQTc
Y477gutEH-a3DBXlLeX7Oj_xuVJ6V-wpxhpFET3XC3458rd151basBE6DcpUvJHZKHIZlTHSC0OUnuuMcYKCZxkoci-b0eOQ
```

This cookie looks like a JWT token. Now, for those who are unfamiliar with it, A JWT (JSON Web Token) is a compact, URL-safe token format used for securely transmitting information between parties as a JSON object. It can be used for authentication and authorization purposes.

It consists of 3 parts: header, payload and secret, each of which is separated by a period. We can verify whether the cookie is a valid token by using the website [jwt.io](https://jwt.io).

![JWT Token decodeer and encoder](https://github.com/user-attachments/assets/1b952f98-e835-42ab-b9e9-7a2eb071003f)

From this, we identify that the algorithm used is RS256 and our current role is 'user'. We need to become 'admin'.

### Attempted Attacks:

1. **None Algorithm Attack:**
    - In the first attack, we check whether the server accepts 'none' algorithm for signing.
    - If that's the case, then we can completely omit the signature part as 'none' algorithm doesn't require a private key for signing. Let's test it out!

    ![None algorithm attack](https://github.com/user-attachments/assets/b49e0e66-3c53-40b0-bcfe-06941a6a9daf)

    - Unfortunately the server rejected it with a 404 error.

    ![Disallowed none alg](https://github.com/user-attachments/assets/af367671-a5ec-4b74-b5c7-9c904a8cf822)


3. **Algorithm Confusion Attack:**
    - This means that the server has already taken precaution against this attack. So let's try something new. Since the usual algorithm, utilises asymmetric cryptography (2 separate keys for encryption and decryption), what if we tried doing a symmetric key attack?
    - Next we try using the confusion attack where we exploit potential misconfigurations of the server so that it mistakenly accept the public key for signing or confuse RS256 with HS256 algorithm (symmetric key algorithm)

    ![Confusion attack](https://github.com/user-attachments/assets/022e37be-ecb9-40e4-823d-5f092b24a4e0)

    - Successfully we access the admin page and revealed the flag.

---

### Flag
```
thiran{JWT_4lg0r1thm_C0nfus10n99^}
```

