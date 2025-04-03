# Bandit05 → 06: Filtering for the Right File

[Challenge](https://overthewire.org/wargames/bandit/bandit6.html)

## Level Description

The task was to find the password stored in a file located somewhere under the `inhere` directory. The file needed to match all the following criteria:

- Human-readable
- Exactly 1033 bytes in size
- Not Executable

## The Process

After logging into the servers as `bandit5`, I started by exploring the home directory:

```bash
$ ls
```

I saw a directory named `inhere`. Knowing the file could be buried anywhere within this directory, I decided to use the `find` command to locate it.

The `find` command is powerful for searching files with specific attributes. I constructed the following command:

```bash
$ find . -type f -size 1033 ! -executable -exec file {} \;
```

- `.`: Start the search in the current directory.
- `-type f`: Limit the search to files only.
- `-size 1033c`: Look for files exactly 1033 bytes in size (`c` stands for bytes).
- `! -executable`: Exclude executable files.
- `-exec file {} \;`: For each file found, execute the `file` command to confirm its type.

The output pointed me to the correct file:

```bash
./inhere/maybehere07/.file2: ASCII text, with very lone lines (1000)
```

Finally, I used the `cat`command to read the file and retrieve the password:

```bash
$ cat ./inhere/maybehere07/.file2
```

Here We go! I got the password for the next level.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Advanced `find` Command Usage**: This challenge taught me how to combine `find` options effectively to locate files with specific properties like size and executability.
- **Combining Commands:** Using `-exec` with `find` allowed me to verify file types without manually inspecting each result.
- **Understanding File Properties:** Recognizing that size, readability, and executability can be filtered was key to solving this challenge.

## Helpful Reading Material

- `man find` - Learn how to search for files with precision.
- `man file` - Understand how to identify file types.
- [Find Command in Linux](https://www.geeksforgeeks.org/find-command-in-linux-with-examples/)
