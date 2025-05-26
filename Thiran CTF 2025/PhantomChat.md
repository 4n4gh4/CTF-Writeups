
### Challenge

We have the website [Phantom Chat](https://thiranctf-phantom-chat.chals.io/) and we need to uncover the hidden flag from it.

---

### Solution

Upon visiting the page, we are directed to a login page. Since we haven't been given any credentials, let's create our own account.

```
Username: newnew
Password: test
```

![Login page](https://github.com/user-attachments/assets/cc56cbe1-c0a1-4f63-975a-274bf6d8cb37)

We login using our credentials and enter the `/chat` page. Here we see that we can chat anonymously using our phantom name with all the users registered in the server. If we click the check privilege button, our current role gets printed.

![Chat interface](https://github.com/user-attachments/assets/868231e7-2c49-42f8-b9ff-21d8afdc3cf5)

Here we see that we can chat anonymously using our phantom name with all the users registered in the server. If we click the check privilege button, our current role gets printed. 

So maybe, our goal is to become the admin. How do we get the privilege? Let's inspect the webpage further.

Upon analysing our interface, we find the following text commented out in `chat.js`

```
actions.sock
    "check_privilege", {};
    "reset_password", { "username":,"new_password:"};
    "ban", {"username"};
    "kick", {"username"};
```

It looks like the website uses Socket.IO to handle privileged actions like Checking privileges (`check_privilege`), Resetting passwords (`reset_password`), Banning users (`ban`) and Kicking users (`kick`).

If we send privileged WebSocket requests, we might be able to escalate our privilege to get admin role. We can try doing this using the console.

First, let us check our privilege through the console, thus printing our username and role on the chat with the following command.

```
socket.emit("check_privilege", {}, (res) => console.log("Privileges:", res));
```

Similarly, let's try to reset the password for 'admin' account with the command we know.

```
socket.emit("reset_password", { "username": "admin", "new_password": "test123" }, (res) => {
    console.log("Reset Password Response:", res);
});
```

Now that we reset the password of user 'admin', let's try logging in as 'admin' to see whether it worked.

Yes! With the new password we set, we are able to login as admin and when we check our privilege, it says we have the admin role. Now what are the possible things we can do on this website with the admin role? Can we access any hidden webpage? Let's try navigating to `/admin`.

![/admin](https://github.com/user-attachments/assets/b37a10ac-f510-4a7a-a272-9122666aac41)

And there you have, the flag is in plain sight!

---

### Flag

```
thiran{W34k_4CTIONS_ACC3SS_C0NTR0L}
```
