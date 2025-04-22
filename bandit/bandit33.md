# Bandit32 → 33: Escaping the UPPERCASE SHELL Trap

[Challenge](https://overthewire.org/wargames/bandit/bandit33.html)

---

## Level Description

You're dropped into a weird shell where every command you type seems to be rejected. Even basic stuff like `ls`, `pwd`, or `whoami` returns "Permissions denied". The goal? is to escape this cursed shell and get the password.

---

## The Process

Alright, so here's what really went down and what you *should* do.

After logged into bandit32, you're now welcomed by a big ASCII banner, but this one is different, it says:

```bash
WELCOME TO THE UPPERCASE SHELL
```

And you're stuck. Everything you type is interpreted ini **all uppercase letters**.

Whenever I try basic stuff commands like `ls`, `pwd`, and `whoami`. It returned: 

```bash
sh: 1: LS: Permission denied
sh: 1: PWD: Permission denied
sh: 1: WHOAMI: Permission denied
```

Yep. the input gets transformed to uppercase; meaning any command you type becomes invalid because Linux commands are case-sensitive (and lowercase by default). So the commands becomes not a valid command.

Remember that we should **not trust the shell we're dropped in**, but instead, try to bypass it/

Use the only available shell trick: the current shell process.

Try this one:

```bash
$0
```

What is `$0`?
- `$0` in shell scripting represents the name of the shell or script being executed.
- In this case, running `$0` will re-execute the shell; but this time, **it doesn't go through the uppercase-filtered wrapper**.

**Boom! You're in a normal** `sh` **shell now**. No more annoying uppercase interference.

Checking what's in the home directory and got this:

```bash
-rwsr-x--- 1 bandit33 bandit32 15140 Apr 10 14:23 uppershell
```

Here's what's interesting:
- `rws` means it has the **SUID bit set**.
- Ownver is **bandit33**, which means if we run this file, it runs **with bandit33's permissions**.
- So, this is our path to escalation.

Let's look closer:

```bash
file uppershell
```

Output:

```bash
uppershell: setuid ELF 32-bit LSB executable, Intel 80386, dynamically linked...
```

Confirmed: it's a binary that runs with bandit33's privileges.

Now, run it:

```bash
./uppershell
```

It drops you **back into the uppercase shell**. But this time, it's not your regular shell; it's running as **bandit33**.

So again, repeat the trick:

```bash
$0
```

You've escaped the uppercase trap again, **but now you're indise as bandit33!** 

Now that you're operating as `bandit33`, just read the password file:

```bash
cat /etc/bandit_pass/bandit33
```

Voila! New Golden Ticket!

---

## Password for the Next Level

`[REDACTED]`

---

## What I Learned

- **Command case matters**. Linux commands are case-sensitive.
- **Shells can be wrapped or restricted** to mess with inputs (like forcing uppercase).
- **$0 is your escape key**. It re-runs the shell binary, which can bypass shell wrappers.
- **SUID binaries** can escalate privileges if used correctly.
- Sometimes the challenge isn't about the tools; it's about tricking the environment.

---

## Helpful Reading Material 

- [What is $0?](https://linuxhandbook.com/bash-dollar-0/)
- [Bash Special Variables](https://linuxhandbook.com/bash-special-variables/)
- [Environment On Linux](https://www.geeksforgeeks.org/environment-variables-in-linux-unix/)
- [Types of Shells on Linux](https://cyberpanel.net/blog/6-types-of-shells-in-linux)

