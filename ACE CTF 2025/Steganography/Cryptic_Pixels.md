## Cryptic Pixels

---

### Challenge

This image looks normal at first, but something important is hidden inside. The secret is carefully concealed, making it hard to find.

Your task is to explore the image, uncover the hidden message, and reveal what’s concealed. Do you have what it takes to crack the code and unlock the secret?

Submit your answer in the following format: ACECTF{3x4mpl3_fl4g}

[CrypticPixels.png](https://acectf.tech/files/7db7c87b1aba00fd7cd4e59c8345b94b/CrypticPixels.png?token=eyJ1c2VyX2lkIjoxMDY4LCJ0ZWFtX2lkIjo2MDAsImZpbGVfaWQiOjUzfQ.Z8PXEw.05FZRtma-DQkmjTaaxNan2JHm7A)

---

### Solution

After downloading our png file, we use binwalk to see whether there's any data embedded in the image.

![binwalk](https://github.com/user-attachments/assets/d4a4a821-5498-4981-84b7-fff7555bcc19)


When we extract the files, we get a password protexted zip `B8103.zip` and a zlib file `5B.zlib`

I'm gonna try cracking the zip password with wordlist `rockyou.txt` using tool `frcrackzip`.

`fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt B8103.zip`

It successfully cracks the password which is: `qwertyuiop`

And, upon entering it we can see that out `flag.txt` got extracted. We just have to `cat` it to see its contents.

`JLNLCO{q4q4_h0d'a3_5v4a7}`

That still looks like it's in an encrypted form. Let's use the [cipher identifier](https://www.dcode.fr/cipher-identifier) to find what kind of encryption it is.

There's a high probability that it is an [ROT cipher](https://www.dcode.fr/rot-cipher). Upon decrypting it, we successfully retrieve the flag.

---

### Flag

```
ACECTF{h4h4_y0u'r3_5m4r7}
