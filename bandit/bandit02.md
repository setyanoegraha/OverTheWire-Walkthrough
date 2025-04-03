# Bandit01 → 02: A Dash of Trouble

[Challenge](https://overthewire.org/wargames/bandit/bandit2.html)

## Level Description

The goal for this level was to find the password for the next level, which was hidden in a file named `-` in the home directory. The tricky part was dealing with the filename, `-` can confuse commands into thinking it’s an option or flag. Once I had the password, I needed to log into `bandit2` using SSH on port `2220`

## The Process

After logging into the servers as `bandit1`, I immediately listed the contents of the home directory:

```bash
$ ls
```

Sure enough, there was a single file named `-` . At first glance, it seemed straightforward to use `cat` to read the file, so I tried:

```bash
$ cat -
```

But instead of showing the file contents, the command seemed to think I was passing a flag, leaving me with an unexpected usage message.

That’s when I remembered: filenames that start with a `-` can confuse commands. The solution was to specify the file path explicitly, so I used `./` to refer to the current directory:

```bash
$ cat ./-
```

And there it was— the password for the next level! I copied the password, then logged into `bandit2` .

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Handling Special Filenames**: Files named with special characters like `-` can confuse commands. Prefixing the file name with `./` tells the command explicitly, “This is a file in the current directory.”
- **Clearing Command Usage Confusion**: When a command doesn’t behave as expected, look closely at the error or usage messages—they can point you toward the solution.
- **Reinforcing File Operations**: Commands like `cat` can have quirks, and understanding how to handle them is key for CTF challenges.

## Helpful Reading Material

- `man cat` – The manual page can help you understand why `-` is treated as a special option.
- [Linux Handling Special Filenames](https://medium.com/@.Qubit/how-to-create-open-find-remove-dashed-filename-in-linux-27ee297d1740)
