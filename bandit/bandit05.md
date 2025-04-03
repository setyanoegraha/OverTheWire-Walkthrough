# Bandit04 → 05: The Search for Readable Data

[Challenge](https://overthewire.org/wargames/bandit/bandit5.html)

## Level Description

The goal for this level was to find the password stored in the only human-readable file inside the `inhere` directory. The trick was to identify which of the multiple files in the directory contained readable text and then extract the password.

## The Process

After logging into the server as `bandit4`, I started by listing the contents of the home directory:

```bash
$ ls
```

I noticed a directory named `inhere`, so I checked its contents:

```bash
$ ls inhere
```

It contained multiple files named `-file00`, `-file01` , and so on. Since the goal was to find the *human-readable* file, I needed a way to determine the type of each file.

I decided to use the `file` command, which identifies the type of content within the files:

```bash
$ file ./inhere/*
```

This revealed the following:

- Most of the files contained data
- One file, `-file07`, was identified as ASCII text (human-readable).

Once I had the file name, I used the `cat` command to read its contents:

```bash
$ cat ./inhere/-file07
```

And there it was—the password for the next level!

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Using the `file`Command:** The `file`command is an excellent tool for determining the type of data within files. It’s especially useful when dealing with files of unknown or mixed content.
- **Interpreting File Types:** Understanding that **ASCII text** indicates human-readable content allowed me to pinpoint the correct file quickly.
- **Efficient Searching:** With multiple files in a directory, it’s important to use tools that can help narrow the search. This strategy is crucial in CTF challenges.

## Helpful Reading Material

- `man file` - Learn more about identifying file types.
- `man ls` - Understand different options for listing files.
