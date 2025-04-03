# bandit00 → 01: Just list

[Level 0 -> Level 1](https://overthewire.org/wargames/bandit/bandit1.html)

## Level Description

The task for this level was straightforward: I needed to find the password for the next level. The hint said the password was stored in a file named `readme` located in the home directory. Once I found it, I had to use the password to log into the next level, bandit1, using SSH like always on port `2220`.

## The Process

After successfully logging into the server as `bandit0`, I found myself staring at the terminal prompt. The first thing that came to mind was: “Let’s check out the home directory.”

I typed `ls` to list the files in the current directory, and sure enough, there was the `readme` file. So far, so good.

Next, I used the `cat` command to peek inside the file:

```bash
$ cat readme
```

And there it was—the password for the next level! The output even congratulated me on my progress, which felt like a nice touch.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **File listing (`ls`)**: Always check your surroundings first! The `ls` command is your best friend for seeing what files are available.
- **Reading Files (`cat`)**: Simple yet powerful, `cat` allows you to quickly view the contents of a file.
- **Logging into Next Levels**: Every password is used to log into the next level using SSH, reinforcing how to string together progress in a sequence.

## Helpful Reading Material

- `man ls` and `man cat` - Get comfortable with these commands; they’ll come up all the time!
