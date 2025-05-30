# Leviathan1 -> 2

Challenge: `leviathan1@leviathan.labs.overthewire.org -p 2223`


## Level Description

Gain access to the password for leviathan2 by exploiting binary called `check` found in **leviathan1's** home directory.

## The Process

Just logging into the server using:
```bash
ssh leviathan1@leviathan.labs.overthewire -p 2223
```

I ran `ls` in the home directory to see if there was anything useful, and I came across a file called `check`.
So, I checked on the file type with `file` command:
```bash
leviathan1@gibson:~$ file check
check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=990fa9b7d511205601669835610d587780d0195e, for GNU/Linux 3.2.0, not stripped
```
This showed that `check` is a **setuid** ELF binary, meaning it runs with the privileges of the owner — likely `leviathan2`.

Next, I used th `strings` command to view printable strings inside the binary:
```bash
strings check
```
Among the outputs, some interesting lines stood out:
```bash
password:
/bin/sh
Wrong password, Good Bye ...
;*2$"0
GCC: (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
crt1.o
```
This hinted that the program compares the user input against some hardcoded string, and might spawn a shell if the password is correct.

To monitor library calls and see how the input is handled, I ran:
```bash
ltrace ./check
```
When prompted for a password, I entered a random string like `y`. The trace showed:
```c
strcmp("y\n\n", "sex")             = 1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
) = 29
+++ exited (status 0) +++
```
The `strcmp()` function compared my input with the string `"sex"`.

From above, it was clear the correct password is `sex`.

So I ran:
```bash
echo "sex" | ./check
```

And got a shell prompt with `sh` shell:
```sh
$
```

Verifying which user I am:
```sh
$ whoami
leviathan2
```
I had succesfully gained a shell as **leviathan2**.

Now, as the `leviathan2` user, I accessed the next level's password:
```sh
cat /etc/leviathan_pass/leviathan2
[REDACTED]
```

## Conclusion
By analyzing the `check` binary using `strings` and `ltrace`, I discovered that is uses `strcmp` to validate the password and that the correct password was hardcoded as `"sex"`. Feeding this value into the program spawned a shell with elevated privileges, allowing me to retrieve the password for **leviathan2**.

## Helpful Reading Material
- [strcmp() - C Library Function](https://cplusplus.com/reference/cstring/strcmp/)
- [Linux `file` command](https://www.geeksforgeeks.org/file-command-in-linux-with-examples/)
- [`strings` Command in Linux](https://labex.io/tutorials/linux-linux-strings-command-with-practical-examples-422934)
- [`ltrace` Command in Linux](https://labex.io/tutorials/linux-linux-ltrace-command-with-practical-examples-422784)
- [How to Pipe Input Using `echo`](https://runcloud.io/blog/echo-command-in-linux)
- [What is a Binary File?](https://en.wikipedia.org/wiki/Binary_file)
- [What Does "Not Stripped" Mean?](https://community.unix.com/t/not-stripped-executable-file/230654)
- [ELF Executables in Linux](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
- [Understanding SetUID and SetGID](https://www.geeksforgeeks.org/setuid-setgid-and-sticky-bits-in-linux-file-permissions/)