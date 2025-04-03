# Bandit10 → 11: Decoding Secrets with Base64
[Challenge](https://overthewire.org/wargames/bandit/bandit11.html)

## Level Description

The password for this next level was stored in the file named **`data.txt`**, which contains `base64` encoded data. Sounds straightforward right? let’s give it a shot

## The Process

Upon logging into as `bandit10` , always check what’s the content of home directory and it’s file type if there it is.

```bash
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ file data.txt
data.txt: ASCII text
```

I’m wondering how long the line of text it will, I typed this command:

```bash
bandit10@bandit:~$ wc -l data.txt
1 data.txt
```

let’s take a look the content within the file:

```bash
bandit10@bandit:~$ cat data.txt

`REDACTE',
It returns a string that We already knew from the level description it was a string with `base64` encoded data. We could use the `base64` command to encode or decode. Don’t forget to check the command description and manual page nor help option using the `whatis` command, `man` command, `[command] --help` .

After reading the `base64 --help` option I could use `-d` option with the file name as an argument to solve this challenge. Pretty simple.

```bash
bandit10@bandit:~$ base64 -d data.txt
The password is [REDACTED] 
```

Voila! Beautiful password has appear!

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Listing Files with `ls` and checking File Type with `file` -** Used `ls` to list files and `file` to determine the type of `data.txt`.
- **Counting Lines in a File with `wc -l` -** Verified that the file contained only one line.
- **Understanding Bae64 Encoding -**  Recognized that the file’s contents were Base64-encoded text.
- **Decoding Base64 with `base64 -d` -** Used the `base64 -d` command to decode the text and reveal the password.
- **Using `man` , `whatis`, and `--help` for learning Commands -**  Consulted these tools to understand the `base64` command and its options.

## Helpful Reading Material

- [base64 encode & decode](https://www.baeldung.com/linux/cli-base64-encode-decode)
- [base64 linux command](https://ioflood.com/blog/base64-linux-command/)
