# Bandit08 → 09: When Something is Unique

[Challenge](https://overthewire.org/wargames/bandit/bandit9.html)

## Level Description

The goal for this level is to find the password stored in the file named `data.txt` which is the only line of the text that occurs only once. Sounds pretty straightforward.

## The Process

After logging in as `bandit7` , like usual:

```bash
$ ls
```

Well, this return file is named `data.txt` and is in the home directory.

I used a command `file` to determine its type:

```bash
$ file data.txt
data.txt: ASCII text
```

I was curious how many lines are there on that file, So I’ve been checking it and it contains 1001 lines.

```bash
$ wc -l data.txt
1001 data.txt
```

With that amount of lines, I knew I was not going to check it manually line-by-line. It’s a time-wasting. I needed a way that was more efficient and effective.

After spending time searching on the website, finding some reading material and I found some commands that I think It’s going to be useful in some way, There are:

- `whatis` command
- `sort` command
- `uniq` command

One thing you have to keep in mind when you encounter a new command that which are foreign to you. You can check the definition of what the command will do by using `whatis` command. For example:

```bash
$ whatis whatis
whatis (1)           - display one-line manual page descriptions
```

From `whatis` command, I got the brief descriptions of `whatis` command because I used the `whatis` command as an argument for it. If I change the argument to `sort` and `uniq` commands:

```bash
$ whatis sort uniq
sort (1)             - sort lines of text files
uniq (1)             - report or omit repeated lines
```

It returns a brief description of `sort` and `uniq` commands. Those two commands are the key to how we are going to find the password for the next level.

I knew I could combine those two commands with piping, first of all, I’m going to sort the file with the `sort` command and then send the output as input using `|` (pipeline) to the `uniq` command with `-u` as a flag because our goal is to find the lines that occur only once.

```bash
$ sort data.txt | uniq -u
[REDACTED]
```

Voila! I’ve got the password for the next level. Easy Win Right?

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Efficient searching techniques -** Instead of manually checking 1001 lines, I learned to automate the process using command-line tools like `sort` and `uniq`.
- **Using the `whatis` command -** Helps quickly understand unfamiliar commands by providing a brief one-line description.
- **Sorting text with `sort` -** Ensures duplicate lines are grouped together, making it easier to filter unique lines.
- **Filtering unique lines with `uniq -u` -**  Extract only lines that appear once, which was key to finding the password.
- **Using pipes `|` for command chaining -**  Allows combining multiple commands efficiently for streamlined data processing.
- **Leveraging Linux utilities for problem-solving -**  Reinforces the power of built-in tools to solve challenges quickly and effectively.

## Helpful Reading Material

- [Piping](https://ryanstutorials.net/linuxtutorial/piping.php)
- [Basic Linux Commands](https://www.geeksforgeeks.org/basic-linux-commands/)
- [Sort Command Linux](https://www.geeksforgeeks.org/sort-command-linuxunix-examples/)
- [Uniq Command in Linux](https://www.geeksforgeeks.org/uniq-command-in-linux-with-examples-2/
- `man whatis`
- `man sort`
- `man uniq`
