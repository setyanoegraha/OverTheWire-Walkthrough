# Bandit26 → 27: 

[Challenge](https://overthewire.org/wargames/bandit/bandit27.html)

---

## Level Description

Good job getting a shell! now hurry and grab the password for bandit27! Now, Can you execute something **with the permissions of another user?**

**NOTE**: For this level for sure is a continuation of the previous level. Since we already got into bandit26, So it's going to pretty much easy to retrieve the password for bandit27. If you already log out from bandit26 and try to logging into bandit26 again, just do the same way like the previous walkthrough (in this case I mean walkthrough for bandit26). The only difference is the walkthrough using sshkey and for now just using the bandit26 password we have already got.

---

## The Process

**Step 1: : Logging into bandit26**

Because I already inside the bandit26 and I've mentioned what have to do in the level description in case you're logged out from this level.

**Step 2: Checking the home directory**

Always check what's inside the home directory for every level you're working on.

```bash
ls
```

And it returned two files named `bandit27-do` and `text.txt`. One regular txt file and the other is binary file.

**Step 3: Check the permissions of bandit27-do**

```bash
ls -l bandit27-do
```

Output:

```bash
-rwsr-x---  1 bandit27 bandit26 14880 Sep 19  2024 bandit27-do*
```

The `s` in `rws` shows that this is a SUID binary — it runs with the **permissions of the file owner**, which is `bandit27`. That's mean if **you run it**, it runs **as if you're bandit27!**.

** Step 3: Run the binary file to print the password**

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Voila! Managed to secure the golden ticket for the next level!.

---

## The password for the next level

`[REDACTED]`

## What I Learned

- **SUID (Set User ID) binaries** can be exploited to run commands **with the file's owner privileges** even if you're not that user.
- A SUID binary can act as a **"bridge" to escalate privileges temporarily**, letting you execute commands as another user.
- You can exploit such binaries by **executing your desired command (like 'cat') as arguments.**

## Helpful Reading Material

- [SUID-SGID-STICKYBIT](https://www.redhat.com/en/blog/suid-sgid-sticky-bit)


