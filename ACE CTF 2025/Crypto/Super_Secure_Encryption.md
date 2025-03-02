
## Super Secure Encryption

---

### Challenge

I'm doing a big favour with this one... I'm handing out my super secure functionality to the outer world to stumble upon & explore. Though, I still remember one of my colleagues once saying that nothing in this world is secure nowadays but my script right here stands on the contrary. I'll give you the access to my arsenal and see if you can prove me wrong.

[chall.py](https://acectf.tech/files/25b1fc092311c09cb812e5063f6f301f/chall.py?token=eyJ1c2VyX2lkIjoxMDY4LCJ0ZWFtX2lkIjo2MDAsImZpbGVfaWQiOjQwfQ.Z8PMkw.EQYl4au6sDgDe2sW1_xGH3KgHYU) + [msg.txt](https://acectf.tech/files/b0fb03529b37bc85143223c68b379257/msg.txt?token=eyJ1c2VyX2lkIjoxMDY4LCJ0ZWFtX2lkIjo2MDAsImZpbGVfaWQiOjQxfQ.Z8PMkw.O23UKM85w1jDsRg4h8svgUpllTE)

---

### Solution

We begin by downloading and inspecting the files.

What the python code `chall.py` does is, it begins by generating a 16 byte key. There is a hint commented out that might allow us to exploit the short length of the key.

Next the AES encryption is defined which uses CTR (counter) mode. This turnes the AES object into a stream cipher. Finally converts the ciphertext into hexadecimal.

There is a sample message in the code and a reference to flag.txt, which possible contains our flag.

In a file titled message.txt, both the encrypted message and flag are saved on different lines.

To reverse all operations and to retrieve our flag, we can use the following script:

#### decrypt.py
```
from binascii import unhexlify

def xor_decrypt(ciphertext, keystream):
    return bytes([c ^ k for c, k in zip(ciphertext, keystream)])

# Known plaintext (from the encryption script)
known_plaintext = b'This is just a test message and can totally be ignored.'

# Read the encrypted messages from msg.txt
with open('msg.txt', 'r') as f:
    encrypted_msg_hex, encrypted_flag_hex = f.read().strip().split("\n")

# Convert hex to bytes
C1 = unhexlify(encrypted_msg_hex)
C2 = unhexlify(encrypted_flag_hex)

# Recover the keystream
keystream = xor_decrypt(C1, known_plaintext)

# Decrypt the flag
decrypted_flag = xor_decrypt(C2, keystream)

print("Recovered flag:", decrypted_flag.decode())

```

---

### Flag

```
Recovered flag: ACECTF{n07h1n6_15_53cur3_1n_7h15_w0rld}
