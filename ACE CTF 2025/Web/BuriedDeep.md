## Binary Search

---

### Challenge

Want to play a game? As you use more of the shell, you might be interested in how they work! Binary search is a classic algorithm used to quickly find an item in a sorted list. Can you find the flag? You'll have 1000 possibilities and only 10 guesses. Cyber security often has a huge amount of data to look through - from logs, vulnerability reports, and forensics. Practicing the fundamentals manually might help you in the future when you have to write your own tools!

**Attached files:**  
[challenge.zip](https://artifacts.picoctf.net/c_atlas/20/challenge.zip)

---

### Solution

We begin by running the webshell provided in picoCTF for basic challenges. Since it is a Linux interface, we need to be familiar with basic Linux commands.

Here, I use `wget` to download the attached file directly from the link:

![Downloading challenge.zip using wget](./attachments/BS1.png)

Now that we have the file in our home directory, we unzip it and find a directory inside it, which leads to further directories until we reach a drop-in directory with `guessing_game.sh`, which is an executable file that prompts us to use the binary search algorithm.

![Extracted files from challenge.zip](./attachments/BS2.png)

#### guessing_game.sh Code

```bash
#!/bin/bash

# Generate a random number between 1 and 1000
target=$(( (RANDOM % 1000) + 1 ))

echo "Welcome to the Binary Search Game!"
echo "I'm thinking of a number between 1 and 1000."

# Trap signals to prevent exiting
trap 'echo "Exiting is not allowed."' INT
trap '' SIGQUIT
trap '' SIGTSTP

# Limit the player to 10 guesses
MAX_GUESSES=10
guess_count=0

while (( guess_count < MAX_GUESSES )); do
    read -p "Enter your guess: " guess

    if ! [[ "$guess" =~ ^[0-9]+$ ]]; then
        echo "Please enter a valid number."
        continue
    fi

    (( guess_count++ ))

    if (( guess < target )); then
        echo "Higher! Try again."
    elif (( guess > target )); then
        echo "Lower! Try again."
    else
        echo "Congratulations! You guessed the correct number: $target"

        # Retrieve the flag from the metadata file
        flag=$(cat /challenge/metadata.json | jq -r '.flag')
        echo "Here's your flag: $flag"
        exit 0  # Exit with success code
    fi

done

# Player has exceeded maximum guesses
echo "Sorry, you've exceeded the maximum number of guesses."
exit 1  # Exit with error code to close the connection
```

---

According to the Binary Search Algorithm, searching is performed by setting the initial index value as `low` and the final index value as `high` for sorted values. In the usual searching algorithm, the program begins by checking whether the key (the number we're searching for) is greater/less than the middle value.

- If it is less, then `middle - 1` is set as the new `high` index.
- Otherwise, `middle + 1` is set as the new `low` index.
- The loop continues while `low <= high`.

This code prompts us to guess a number where we have only 10 chances to get it right. By visualizing the binary search algorithm, we can make guesses and find the flag.

We use the `SSH` (Secure Shell) command given in the challenge description to remotely connect to the server, which will return the flag. Upon entering the password, the same `guessing_game.sh` runs on the server, and upon making the correct guesses, we retrieve our flag!

![Using ssh to get the flag](./attachments/BS3.png)

---

### Flag

```
picoCTF{g00d_gu355_de9570b0}
