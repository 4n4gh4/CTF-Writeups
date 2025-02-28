## Buried Deep

---

### Challenge

You know what's a bucketlist? In simple terms, it's just a list of wishes people want to achieve before the leavee this world. I found it to be very limiting & ironic because how can you know when you'll leave the world behind? It's better to enjoy every moment and take on every opportunity you can. One of my whishes though is to pet a cat, do you mind checking this one out. So cute.

[What a cutie patootie!](https://opening-account-acectf.s3.ap-south-1.amazonaws.com/fun/can_we_get_some_dogs/026.jpeg)

---

### Solution

The website gives us the picture of a cat which is quite random. I tried to look for more images in the same folder `can_get_some_dogs` but gave up after searching from 001.jpeg to 030.jpeg. The only other image I found 

Next, I ventured back directory by directory and ended up in root `https://opening-account-acectf.s3.ap-south-1.amazonaws.com/`

Here, there seemed to be a list of files printed. I began looking at the file names and files for possible clues.

Came across a file titles hint.txt... Then it struck me to maybe search whether there are more .txt files. I used `Ctrl+F` to search and found another .txt file titled secret.txt.

I navigated to the file and it printed a clear base64 encoded text. Upon decoding it, I got the flag.

---

### Flag

```
ACECTF{7h3_4w5_15_m15c0nf16ur3d}
