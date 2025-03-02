## Broken Secrets

---

### Challenge

You’ve found a suspicious file, but it seems broken and cannot be opened normally. Your goal is to uncover its secrets.

Submit your answer in the following format: ACECTF{3x4mpl3_fl4g}

[Brokenfr](https://acectf.tech/files/ccfa5888f45831d567c2cdb8639df526/Brokenfr?token=eyJ1c2VyX2lkIjoxMDY4LCJ0ZWFtX2lkIjo2MDAsImZpbGVfaWQiOjQ1fQ.Z8PfcQ.TRkXHWjBdrhPdEx7OWWvaD34gdo)

---

### Solution

Upon using `file` command, we find that the given file is a 7-zip archive data. We extract the contents using the following command.

`7z x Brokenfr`

The extracted contents are present in a firectory `_`. We explore the directory for further clues. In the `_/word/media` folder we find a `not_so_suspicious_file` which comprises of data. This is exactly what makes it suspicious.

Another thing I noticed was, in the `_/word/_rels` folder, we have a `document.xml.rels` file. And when I inspected it, there is a reference to an image1.png in the media folder.

![document.xml.rels](https://github.com/user-attachments/assets/9096cab1-0046-4729-9061-3e3e0b868db9)

This means that an image1.png is supposed to exist in the media folder, but now there's a very suspicous file in its place. As the title suggest, what if the image1.png is the broken one, and we have to retrive it?

To do this, I navigated back to the media folder and used hexedit on `not_so_suspicious_file`

A proper png file will have the magic bytes: 89 50 4E 47 0D 0A 1A 0A

But here, the first 4 bytes are differnt. Let's fix it to proper png header. After this, use file command and we see that our data file has been converted to a png file. Try opening it using `display` command, and we'll see our flag.

---

### Flag

```
ACECTF{h34d3r_15_k3y}
