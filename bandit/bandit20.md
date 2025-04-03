[Challenge](https://overthewire.org/wargames/bandit/bandit20.html)

## Level Description

The password for the next level is stored in **`/etc/bandit_pass/bandit20`**. However, I cannot access it directly as **`bandit19`** user. Instead, I found the **setuid binary** named **`bandit20-do`** in the home directory. This binary allows me to execute commands as the **`bandit20`** user, which I can use to retrieve the password.

## The Process

**Step 1: Logging into Bandit19**

I logged into the Bandit19 server using SSH:

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

After entering the password, I was in.

**Step 2: Exploring the Home directory**

I listed the contents of the home directory:

```bash
ls
```

This revealed a single file:

```bash
bandit20-do
```

**Step 3: Understanding the Setuid Binary**

I checked the file type and permissions using the **`file`** and **`ls -l`** commands:

```bash
file bandit20-do
```

Output:

```bash
bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=368cd8ac4633fabdf3f4fb1c47a250634d6a8347, for GNU/Linux 3.2.0, not stripped
```

```bash
ls -l bandit20-do
```

Output:

```bash
-rwsr-x--- 1 bandit20 bandit19 14880 Sep 19 07:08 bandit20-do
```

The **`s`** in the permissions (**`rws`**) indicates that this is a **setuid binary.** This means the binary runs with the privileges of the file owner (**`bandti20`**), not the user executing it (**`bandit19`**).

**Step 4: Exploiting the Setuid Binary**

I used the **`bandit20-do`** binary to execute commands as the **`bandit20`** user. To retrieve the password for Bandit20, I ran:

```bash
bandit20-do cat /etc/bandit_pass/bandit20
```

The output was the password for Bandit20:

```bash
[REDACTED]
```

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Setuid Binaries:** I learned how setuid binaries work and how they can be used to execute commands with the privilege of the file owner.
- **Privilege Escalations:** This level demonstrated how to exploit setuid binaries for privilege escalation.
- **File Permissions:** I gained a deeper understanding of file permissions and the significance of the **`setuid`** bit.

## Helpful Reading Material

- [Setuid and Setgid on Linux](https://linux.die.net/man/2/setuid): A guide to understanding setuid and setgid permissions.
- [Linux File Permissions](https://linux.die.net/man/1/chmod): Documentation on file permissions and how to modify them.
