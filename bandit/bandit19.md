# Bandit18 → 19: Bypassing the Logout Trap

[Challenge](https://overthewire.org/wargames/bandit/bandit19.html)

## Level Description

The password for the next level is stored in a file named **`readme`** in the home directory. However, someone has modified the **`.bashrc`** file to log me out immediately upon logging in via SSH. To retrieve the password, I needed to find a way to bypass this restriction and access the **`readme`** file.

## The Process

**Step 1: Understanding the Problem**

When I attempted to log into Bandit18 using SSH, I was immediately logged out. This behaviour was caused by a modification to the **`.bashrc`** file, which runs automatically upon login. To bypass this, I needed to execute commands without triggering the **`.bashrc`** file.

**Step 2: Using SSH to Execute Commands Directly**

Instead of logging into a shell, I used SSH to execute commands directly. This bypasses the **`.bashrc`** file and allows me to interact with the server.

First, I listed the contents of the home directory:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 ls
```

The output showed a single file:

```bash
readme
```

**Step 3: Retrieving the Password**

Next, I used SSH to read the contents of `readme` file:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat ./readme
```

The output revealed the password for Bandit19:

```bash
[REDACTED]
```

**Step 4: Alternative Approach (Using `bash` or `sh`)**

To confirm the password, I also tried logging in using a different shell (`bash` or `sh`) to bypass the **`.bashrc`** file. This allowed me to interact with the server directly:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 bash
```

Once logged in, I navigated the directoy and confirmed the password:

```bash
ls
cat readme
```

The output was consistent:

```bash
[REDACTED]
```

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Bypassing Restrictions:** I learned how to bypass automatic logout mechanisms by executing commands directly via SSH or using alternative shells.
- **SSH Command Execution:** I gained experience using SSH to run commands remotely without logging into an interactive shell.
- **Troubleshooting:** This level reinforced the importance of understanding how shell intialization files (like **`.bashrc`**) work and how to work around them.

## Helpful Reading Material

- [SSH Command Execution](https://linux.die.net/man/1/ssh): Documentation on using SSH to execute commands remotely.
- [Bash Startup Files](https://www.gnu.org/software/bash/manual/bash.html#Bash-Startup-Files): A guide to understanding how `.bashrc` and other startup files work.
