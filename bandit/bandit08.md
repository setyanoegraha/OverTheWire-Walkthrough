# Bandit07 → 08: Searching Smart, Not Hard

[Challenge](https://overthewire.org/wargames/bandit/bandit8.html)

## Level Description

The goal of this level is to locate a password hidden inside a file named `data.txt`. The password is positioned next to the word `millionth`.

However, the challenge is that `data.txt` is a massive file with nearly 100,000 lines, making manual inspection impractical. This requires an efficient way to extract the required information.

## The Process

Upon logging into as `bandit7`, I ran:

```bash
$ ll -a
```

This command lists all files, including hidden ones in a long listing format. The output revealed a **`data.txt`.**

To understand the file format, I used the `file` command:

```bash
$ file data.txt
data.txt: Unicode text, UTF-8 text
```

This tells us that the file contains **text data** and is encoded in **UTF-8 format.**

To get a sense of how large this file is, I counted the number of lines:

```bash
$ wc -l data.txt
98567 data.txt
```

With 98,567 lines, manually scanning through the file is out of the question.

Since we know the password is next to the word **millionth**, we can use `grep` to extract the relevant line:

```bash
$ cat data.txt | grep millionth
```

Voila! The password is the string next to “millionth”

```bash
millionth       `[REDACTED]`
```

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Using `file`** to check file types and encodings.
- **Using `wc -l`**  to count the number of lines in a file
- **Using `grep`** to search for specific text inside large files.

## Helpful Reading Material

- `man ls`
- `man grep`
- `man wc`
- [Grep Command in Linux](https://www.geeksforgeeks.org/grep-command-in-linux-with-examples/)
- [Piping in Linux](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)
- [Basix Linux Commands](https://www.geeksforgeeks.org/basic-linux-commands/#25-wc-command-in-linux)
- [Piping](https://ryanstutorials.net/linuxtutorial/piping.php)
