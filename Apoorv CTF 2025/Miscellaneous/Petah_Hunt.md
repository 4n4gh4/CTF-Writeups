## Petah Hunt

---

### Challenge

Bro, who is this guy everyone’s talking about? He’s known for his shady antics and mastery of hidden secrets. Check out the image and see if you can uncover what he’s hiding.

[petah.png](https://apoorvctf.iiitkottayam.ac.in/files/a031869ad55ab0d8ae91323159de4b6d/petah.png?token=eyJ1c2VyX2lkIjoxMTk4LCJ0ZWFtX2lkIjo1OTcsImZpbGVfaWQiOjQ5fQ.Z8VCjA.5pagw0U29PUnYd4oLvmDdG88dqM)

---

### Solution

I began by downloading the file, and inspecting it with `exiftool` to find a comment `username: buffedpetah` which is probably a very critical clue.

Upon using `binwalk`, I realised there's a zlib compressed file in the png and I extracted it. To be honest, I might have spent almost an hour just inspecting the compressed data but to no avail.

When I thought about it, since it's a misc challenge, multiple domains could be mixed in this... probably. And that prompted me to google up `buffedpetah` and bingo! I found a github repo which was created fairly recently which had the profile same as the given png file.

![buffedpetah](https://github.com/user-attachments/assets/c33924f8-55f4-47a8-bf62-05b8076c5e10)

The [readme](https://github.com/buffedpetah) file translates to:

```
Hello, I'm Peter Griffin. Student by day, Fortnite warrior by night. I enjoy burgers that might be portals to other dimensions, and the weird moments life throws at me. I once watched my horse attempt to moonwalk under a double rainbow. I stoically surrender to the cosmic absurdity of everyday life.

Look through my files and you might find something interesting.

I'm a fool, but I'm also a genius.
```

This was very suspicious so I confirmed with the admin whether this account was part of the challenge, and I was right.

Upon exploring the repositories, I found something weird. `FreakQuest` appears to be a project that `buffedpetah` is working on. The python script and readme file suggest that it is an experimental project that generates and analyzes a synthetic audio signal composed of random sine waves. It aims to explore hidden mysteries within unexpected frequency patterns.

I wasn't sure how to proceed with it because I didn't have any audio file to process. But then, I investigated the `project.obj` file. Upon asking an LLM, it clarified that the file consisted of multiple freqeuncy values in the form `v x y z` where `x` and `y` are the frequency ranges and `z` is the duration.

It was actually a DTMF code with duration. Now for those who are unfamiliar, DTMF (Dual-Tone Multi-Frequency) is a signaling system used for telecommunication signaling over telephone lines. It generates specific audio signals (tones) when you press the keys on a telephone keypad. Each key press produces a combination of two frequencies, one from a high-frequency group and one from a low-frequency group. It's the same thing that we see on our phone keypad where each digit pressed gives a different sound.

I decoded the DTMF code but realised that all of it seemed... jumbled. How do I fix it?

Since the 3rd set of values describe the duration, I used it like an index value to reorder the frequencies. After reordering, I decoded it usingg DTMF mapping. Here's the script I wrote for it:

```
# DTMF tone mapping
dtmf_mapping = {
    (697, 1209): "1", (697, 1336): "2", (697, 1477): "3",
    (770, 1209): "4", (770, 1336): "5", (770, 1477): "6",
    (852, 1209): "7", (852, 1336): "8", (852, 1477): "9",
    (941, 1209): "*", (941, 1336): "0", (941, 1477): "#"
}

# Input data
data = [
    (941, 1209, 8200), (941, 1209, 9900), (852, 1477, 10900),
    (697, 1477, 5200), (941, 1477, 11100), (770, 1209, 4200),
    (697, 1336, 10400), (941, 1209, 6300), (852, 1336, 1800),
    (941, 1209, 2000), (852, 1336, 10700), (941, 1209, 1600),
    (697, 1477, 9700), (852, 1209, 8900), (941, 1209, 4400),
    (770, 1209, 4300), (770, 1477, 1000), (770, 1209, 5000),
    (941, 1209, 5400), (770, 1209, 5700), (852, 1209, 1500),
    (852, 1209, 300), (770, 1477, 6200), (852, 1209, 6400),
    (941, 1209, 10800), (941, 1209, 4600), (770, 1477, 1100),
    (697, 1477, 3600), (770, 1209, 9500), (941, 1209, 11000),
    (697, 1336, 2200), (852, 1209, 6600), (941, 1209, 800),
    (852, 1209, 7300), (697, 1336, 2100), (941, 1209, 8600),
    (941, 1209, 3200), (852, 1209, 3300), (770, 1477, 900),
    (941, 1209, 3900), (852, 1336, 1900), (941, 1209, 7200),
    (852, 1209, 7400), (770, 1209, 9400), (941, 1209, 10100),
    (941, 1209, 2400), (941, 1209, 5100), (941, 1209, 9300),
    (941, 1477, 9200), (941, 1209, 4100), (770, 1477, 500),
    (941, 1209, 2600), (852, 1336, 3800), (941, 1209, 10600),
    (770, 1209, 8300), (941, 1209, 9100), (941, 1209, 7700),
    (852, 1209, 7000), (697, 1477, 5300), (852, 1209, 8800),
    (852, 1209, 7100), (852, 1209, 9000), (770, 1477, 6000),
    (697, 1477, 2700), (770, 1477, 6100), (941, 1477, 10200),
    (770, 1477, 700), (770, 1477, 600), (697, 1336, 2300),
    (852, 1209, 1300), (941, 1209, 9600), (852, 1209, 7600),
    (941, 1209, 4800), (852, 1336, 1700), (852, 1336, 4700),
    (941, 1209, 3400), (941, 1477, 4500), (697, 1477, 7800),
    (852, 1209, 6500), (941, 1209, 1200), (941, 1209, 5600),
    (941, 1209, 6800), (941, 1209, 8000), (852, 1209, 6700),
    (697, 1477, 7900), (941, 1209, 10300), (941, 1209, 200),
    (852, 1209, 6900), (697, 1477, 9800), (770, 1209, 8400),
    (697, 1477, 3500), (852, 1336, 2500), (770, 1209, 4900),
    (852, 1209, 8700), (941, 1209, 400), (697, 1336, 10000),
    (941, 1209, 3700), (852, 1209, 1400), (697, 1336, 10500),
    (770, 1209, 8500), (697, 1477, 2900), (697, 1336, 100),
    (852, 1209, 7500), (770, 1209, 5800), (941, 1209, 5900),
    (941, 1209, 3000), (941, 1477, 3100), (941, 1477, 8100),
    (697, 1336, 4000), (697, 1477, 2800), (941, 1477, 5500)

]

# Extract keys
keys = [dtmf_mapping[(v1, v2)] for v1, v2, v3 in data]

# Sort or filter based on value3 if needed
# Example: Sort by value3
sorted_keys = [key for _, key in sorted(zip([v3 for _, _, v3 in data], keys))]

# Print the result
print("".join(sorted_keys))
```

And I got the perfect output `2*7*666*666*777*888*222*8*333*#*7*33*8*2*44*#*8*44*33*#*44*666*7777*777*7777*33*#*444*7777*#*44*33*2*#*22*8*9*#`

I converted this to normal letters and replaced the hashtages with `{` and `_`, also there was a typo in between and after fixing that, I got the flag!

---

### Flag

`apoorvctf{petah_the_hosrse_is_hea_btw}`
