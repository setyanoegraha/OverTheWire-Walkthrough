# Leviathan3 -> 4

Challenge: `leviathan3@leviathan.labs.overthewire.org -p 2223`

## Level Description

locate the password for the next level by analyzing the binary file named `level3` located in the home directory.

## The Process

As usual, I checked the home directory and found file named `level3`:
```bash
leviathan3@gibson:~$ ls
level3
```

next, I inspected the file details:
```bash
leviathan3@gibson:~$ ll -a level3
-r-sr-x--- 1 leviathan4 leviathan3 18100 Apr 10 14:23 level3*
```
It turns out to be a **setuid binary**, owned by `leviathan4`, meaning it runs with the privileges of that user.

I Tried running the binary to observe its behaviour:
```bash
leviathan3@gibson:~$ ./level3
Enter the password> yalow
bzzzzzzzzap. WRONG
```
As expected, the password was incorrect. I just wanted to see how the binary responds.

Then, I used `ltrace` to trace binary calls:
```bash
leviathan3@gibson:~$ ltrace ./level3
__libc_start_main(0x80490ed, 1, 0xffffd494, 0 <unfinished ...>
strcmp("h0no33", "kakaka")                   = -1
printf("Enter the password> ")               = 20
fgets(Enter the password> nothing
"nothing\n", 256, 0xf7fae5c0)          = 0xffffd26c
strcmp("nothing\n", "snlprintf\n")           = -1
puts("bzzzzzzzzap. WRONG"bzzzzzzzzap. WRONG
)                   = 19
+++ exited (status 0) +++
```
This time I entered `"nothing"` as the input. From the trace, it became clear that the user input is being compared to the string `"snlprintf\n"` using `strcmp`. That revealed the actual password.

I then tried using the discovered string:
```bash
leviathan3@gibson:~$ ./level3
Enter the password> snlprintf
[You've got shell]!
```
I got a shell! To confirm the privilege escalation:
```sh
$ whoami
leviathan4
```
Success! now i could read the password directly:
```sh
$ cat /etc/leviathan_pass/leviathan4
[REDACTED]
```
On to the next one!

## Conlusion
In this leve, we encountered a classic example of privilege escalation via a setuid binary. By analyzing the binary using `ltrace`, we were able to identify the hardcoded password it compares user input againts. This simple yet effective dynamic analysis technique allowed us to bypass the authentication check and gain a shell as the next user, `leviathan4`. Once inside, accessing the next level's password was straightforward. This challenge highlights how tools like `ltrace` can reveal internal program logic without needing to reverse engineer the binary statically.

## Helpful Reading Material
- [strcmp() Function in C](https://cplusplus.com/reference/cstring/strcmp/#google_vignette)
- [ltrace man page](https://man7.org/linux/man-pages/man1/ltrace.1.html)
- [SetUID, SetGID, and Sticky Bits in Linux](https://www.geeksforgeeks.org/setuid-setgid-and-sticky-bits-in-linux-file-permissions/)