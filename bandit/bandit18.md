# Bandit17 → 18: Finding the Needle in the Haystack

[Challenge](https://overthewire.org/wargames/bandit/bandit18.html)

## Level Description

There are two files in the home directory. **`passwords.old`** and **`passwords.new`**. The password for the next level is in **`passwords.new`** and is the only line that has been changed between **`passwords.old`** and **`passwords.new`**.

**NOTE:** if you have solved this level and see ‘Byebye!’ when trying to log into Bandit18, this is related to the next level, Bandit19.

## The Process

**Step 1: Logging into Bandit17**

After successfully retrieving the password for this level, as I’ve explained in previous walkthrough, I used it to log into Bandit17:

```bash
ssh bandit17@bandit.labs.overthewire.org -p 2220
```

Once logged in, I checked the contents of the home directory:

```bash
ls
```

This revealed two files:

```bash
passwords.old passwords.new
```

**Step 2: Understanding the Task**

The goal was to find the password for Bandit18, which was the only line that had changed between **`passwords.old`** and **`passwords.new`**. To do this, I needed to compare the two files and identify the difference 

**Step 3: Using `diff` to compare files**

I used to **`diff`** command to compare the two files:

```bash
diff passwords.old passwords.new
```

The output showed the difference between the files:

```bash
42c42
< ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB
---
> [REDACTED]
```

This indicatd that line 42 in **`passwords.old`** was replaced with a new line **`passwords.new`**.

**Step 4: Extracting the Password**

The new line in **`passwords.new`** was the password for Bandit18.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **File Comparison**: I learned how to use the `diff` command to compare two files and identify differences.
- **Attention to Detail**: This level reinforced the importance of carefully examining output to find the relevant information.
- **Efficiency**: Using the right tool (`diff`) made the task quick and straightforward.

## Helpful Reading Material

- [diff Command Documentation](https://www.gnu.org/software/diffutils/manual/diffutils.html): A comprehensive guide to using `diff` for file comparison.
- [Linux File Comparison Tools](https://linux.die.net/man/1/diff): An overview of tools for comparing files in Linux.
