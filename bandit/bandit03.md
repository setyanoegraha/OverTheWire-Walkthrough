# Bandit02 → 03: Mind the Spaces

[Challenge](https://overthewire.org/wargames/bandit/bandit3.html)

## Level Description

The goal for this level was to retrieve the password stored in a file named `spaces in this filename` located in the home directory. The challenge here was dealing with spaces in the filename, which required special handling when using commands.

## The Process

After logging into the server as `bandit2` , I listed the contents of home directory:

```bash
$ ls
```

There it was—a file named `spaces in this filename`. I knew that filenames containing spaces can’t just be typed directly into commands without either quoting them or escaping the spaces.

To read the file, I had two options:

1. Use single quotes:
    
    ```bash
    $ cat 'spaces in this filename'
    ```
    
2. Escape the spaces with backslashes:
    
    ```bash
    $ cat spaces\ in\ this\ filename
    ```
    

Both commands worked, revealing the password for the next level.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Handling Filenames with Spaces**: When dealing with filenames that include spaces, quoting the filenames (e.g., `'filename with spaces'`) or escaping the spaces (e.g., `filename\ with\ spaces`)is essential.
- **File Operations:** This challenge reinforced the importance of understanding how to handle unconventional file names in Linux.

## Helpful Reading Material

- `man cat` - Learn more about the `cat` command for reading files.
- [Filename Spaces Linux](https://linuxhandbook.com/filename-spaces-linux/)
