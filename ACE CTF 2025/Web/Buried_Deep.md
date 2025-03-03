## Buried Deep

---

### Challenge

"I’m not a hacker. I’m just someone who wants to make the world a little better. But the world isn’t going to change itself."

Submit your answer in the following format: ACECTF{3x4mpl3_fl4g}

The flag content should be in lowercase letters only.

[](http://34.131.133.224:9998/)

---

### Solution

The website has a lot of philosophical text. Since the clue is 'Buried Deep' let's look at the robots.txt of the website.

![robots.txt](https://github.com/user-attachments/assets/3f5c61b5-82c7-4656-87d0-43d1bc0a0eb7)

After examining each file in the robots.txt, we find these following clues.

![/buried](https://github.com/user-attachments/assets/77fa21cf-e7ee-4c6d-b456-088d8b18cb3c)

![/secret_path](https://github.com/user-attachments/assets/62b47317-09e5-429b-8dc6-7fbb9e6980fe)


![/encrypted](https://github.com/user-attachments/assets/b3bd9e76-0137-4b38-8c0d-3c8f33602bf5)

We copy the ascii text from /buried, and decode it to get: `1st Part of the Flag is : ACECTF{1nf1l7r471ng_7h3_5y573m_`

We copy the morse code from /secret_path and decode it to get `2NDPARTOFTHEFLAGIS:15_345Y_WH3N_Y0U_KN0W_WH3R3_`

For the 3rd part, as the clue suggest, we check out the css file present for styling, and find this code:

#### styles.css Code
```
#flag {
    display: none;
    content: "bC5 !2CE @7 E96 u=28 :D i f9b0db4CbEd0cCb03FC`b5N"; 
}
```

if we paste this text in [cipher identifier](https://www.dcode.fr/cipher-identifier) it tells us that the text is probably ROT47 encoded. We use the decoder to get the finaly part of the flag: `3rd Part of the Flag is : 7h3_53cr3t5_4r3_bur13d}`

Finally, we put everything together and make it lowercase.

---

### Flag

```
ACECTF{1nf1l7r471ng_7h3_5y573m_15_345y_wh3n_y0u_kn0w_wh3r3_7h3_53cr3t5_4r3_bur13d}
