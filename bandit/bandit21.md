# Bandit20 → 21: Network and Setuid Exploitation

[Challenge](https://overthewire.org/wargames/bandit/bandit21.html)

---

## Level Description

The password for the next level is stored in a file accessible only by the **`bandit21`** user. To retrieve it, I needed to use a **setuid binary** named **`suconnect`** in the home directory. This binary connects to a specified port on **`localhost`**, reads a line of text, and compares it to the password for bandit20. If the password is correct, it transmits the password for bandit21.

---

## The Process

**Step 1: Logging into bandit20**

I logged into the bandit20 server using SSH:

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

After entering the password, I was in.
**Step 2: Exploring the Home Directory**

I listed the contents of the home directory:

```bash
ls
```

This revealed a single file:

```bash
suconnect
```

**Step 3: Understanding the Setuid binary**

I checked the file type and permissions using the **`file`** and **`ls -l`** commands:

```bash
file suconnect
```

Output:

```bash
suconnect: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=4c95669a71860e303b714721dde9020213ad3c9a, for GNU/Linux 3.2.0, not stripped
```

```bash
ls -l suconnect
```

Output:

```bash
-rwsr-x--- 1 bandit21 bandit20 15604 Sep 19 07:08 suconnect
```

The **`s`** in the permissions (**`rws`**) indicates that this is a **setuid binary**. This means the binary runs with the privileges of the file owner (**`bandit21`**), not the user executing it (**`bandit20`**).

**Step 4: Running the Binary**

I ran the **`suconnect`** binary to understand its usage:

```bash
./suconnect
```

Output:

```bash
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

**Step 5: Setting Up a Listener**

To interact with the **`suconnect`** binary, I needed to set up a listener on a specific port. I used **`nc`** (Netcat) to listen on port **`3445`** and send the bandit20 password:

```bash
echo -n "[REDACTED]" | nc -l -p 3445 &
```

This command runs **`nc`** in the background, listening on port **`3445`** and sending the passworf when a connection is made.

**Step 6: Connecting with `suconnect`**

While the listener was running, I executed the **`suconnect`** binary to connect to the same port:

```bash
./suconnect 3445
```

The output confirmed that the password was correct:

```bash
Read: [REDACTED] 
Password matches, sending next password
[REDACTED]
```

Voila! There it is our’s beautiful password!

## Password for the Next Level

`[REDACTED]`

---

## What I Learned

- **Setuid Binaries**: I reinforced my understanding of setuid binaries and how they can be used to execute commands with elevated privileges.
- **Networking with Netcat**: I learned how to use `nc` to set up a listener and send data over a network connection.
- **Inter-Process Communication**: This level demonstrated how two processes can communicate over a network socket to achieve a specific goal.

---

## Helpful Reading Material

- [Netcat (nc) Command](https://linux.die.net/man/1/nc): Documentation on using Netcat for networking tasks.
- [Setuid and Setgid on Linux](https://linux.die.net/man/2/setuid): A guide to understanding setuid and setgid permissions.

---

This level was a great exercise in combining networking and privilege escalation techniques. By setting up a listener and using the `suconnect` binary, I was able to retrieve the password and move forward. Onward to Bandit21! 🚀
