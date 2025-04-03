# Bandit03 → 04: Hidden in Plain Sight

[Challenge](https://overthewire.org/wargames/bandit/bandit4.html)

## Level Description

The goal for this level was to find the password for the next level, hidden in a file within the `inhere` directory. The catch? The file was hidden, so I needed to use commands that could reveal it.

## The Process

After logging in as `bandit3`, I started by listing the contents of the home directory:

```bash
$ ls
```

This revealed a single directory named `inhere` . Based on the goal, I knew the hidden file was somewhere inside this directory, so I decided to investigate further.

To reveal hidden files, I used the `-a` flag with the `ls` command:

```bash
$ ls -al ./inhere
```

This command listed all files in the `inhere` directory with long listing format, including the hidden ones. Sure enough, there it was—a file named `...Hiding-From-You`.

Next, I read the contents of the file using `cat`:

```bash
$ cat inhere/...Hiding-From-You
```

The password for the next level appeared!

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Hidden Files**: Files that begin with a `.` are considered hidden in Linux. To list them you need the `-a` (all) or `-al` (all with details long listing format) flag with `ls`.
- **File Permissions:** Observing file permissions in `ls -al` output helped confirm the file could be read by my user.
- **Exploration Skills**: This challenge reinforced the importance of checking for hidden files and directories when solving CTF puzzles.

## Helpful Reading Material

- [View and Create Hidden Files](https://www.geeksforgeeks.org/how-to-view-and-create-hidden-files-in-linux/)
- `man ls` - learn more about listing files, especially with hidden ones.
