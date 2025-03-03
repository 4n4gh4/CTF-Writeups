## Phantom Connection

---

### Challenge

Like a fleeting dream, a connection once existed but has faded into the void. Only shadows of its presence remain. Can you bring it back to light?

[files.zip](https://apoorvctf.iiitkottayam.ac.in/files/2bbf7c6bfd070e9bd900b7089c03d124/files.zip?token=eyJ1c2VyX2lkIjoxMTk4LCJ0ZWFtX2lkIjo1OTcsImZpbGVfaWQiOjIxfQ.Z8VMow.bm69HNLez1e5Zy-lxRuf83_Uq2w)

---

### Solution

After downloading and unzipping the file, I found a `Cache0000.bin` and an empty .bmc file. Since there is .bmc involved, I'm not exactly familiar with it so I searched online for forensic tools. Here's a cool find I made: [bmc-tools](https://github.com/ANSSI-FR/bmc-tools)

Since we have a .bmc file (even if it's empty) and a `Cache0000.bin`, it suggests that the challenge involves browser cache forensics, and this tool is very useful for it.

After downloading the tool and navigating to the direcotry `bmc-tools`, I ran the following command to extract data from the `Cache0000.bin`

![image](https://github.com/user-attachments/assets/0628638c-5ba2-4885-8f2d-15669e23d371)

Here, the extracted 1980 files are now stored in me `neww` directory. These files are all apparently .bmp files. I used `binwalk` on the files but didn't find anything usefule. The images seemed to be normal .bmp files. But since there are so many of them, what if they are are pieces of something big?

I tried combining all the pictures together by using `montage *.bmp -tile 45x44 -geometry +0+0 output.bmp` and got this output:

![image](https://github.com/user-attachments/assets/1118f345-22f7-4568-8e96-3f90df74c985)

From this we can see parts of the flag. I zoomed in and connected all the pieces together to get the final flag.

---

### Flag

```
apoorvctf{CAcH3_Wh4T_YoU_SE3}
