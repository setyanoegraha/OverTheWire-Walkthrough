# Bandit25 → 26: Escaping the Pager Trap, Breakout Through Vim

[Challenge](https://overthewire.org/wargames/bandit/bandit26.html)

---

## Level Description

Logging in to bandit26 from bandit25 should be fairly easy... The shell for user bandit26 isn't **/bin/bash**, but something else. We are going to find out what is the shell first and then how it works as well as how to break out of it.

**NOTE**: In case you're a Windows user and typically using use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should you command prompt instead.

---

## The Process

**Step 1: Logging into bandit25**

As usual, Connected to the bandit server using SSH:

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

Once inside, there was a ssh key for bandit26 in home directory with named `bandit26.sshkey`.

**Step 2: Try logging in to bandit26**

Since I got the ssh key, I'm goint to use the `-i` option with the provided sshkey file.

```bash
ssh -i bandit26.sshkey bandit26@localhost -p 2220
```

At first I thought this going to be easy but it turns out the server immediately closed after I connected.

**Step 3: Checking the shell**

I need to check what shell the bandit26 used.

```bash
cat /etc/passwd | grep bandit26
```

It returned:

```bash
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

Well, The shell was changed to run a script named `showtext`.

Time to see what's inside the `showtext`:

```bash
cat /usr/bin/showtext
```

So, It returned:

```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

Explanation:

`#!/bin/sh`

This tells the system to use the **sh shell** to run the script.

`export TERM=linux`
It sets the terminal type to **linux**, so the program knows how to show text on screen properly.

`exec more ~/text.txt`

This is the main part:

- It shows the content of the file `text.txt` using the `more` command (one page at a time).
- `exec` means it **replaces the shell with** `more`, so there's **no normal shell** running anymore.
- If **your terminal is big enough to show the full file**, `more` doesn't need to **paginate** — it **just shows the file and exits**, which means, You immediately **get logged out** and looks like nothing happened.
- BUT if you **shrink you terminal window** so the file **doesn't fit on the screen**, `more` command will be appear.

`exit 0`

This is here just in case, but it **never runs**, because `exec` already ended the shell.

So, What actually happens when try to log in?

- You **don't get a normal terminal** like bash.
- Instead, you only see the `text.txt` **file** if your terminal is small enough, otherwise it quits immediately.
- You **can't type commands**, because you're not in a shell, you're stuck in `more`.
- It's a way to **limit what you can do** - kind of like being trapped in a "read-only mode".

**Step 4: Shrinking the terminal and Get into Vim**

Before try connecting to bandit26 with ssh key. Minimizes the terminal to be small enough, so it will get interative pager when you inside the `text.txt` file.

Inside this type `v`, So we can enter vim editor as a route to escape.
 
Inside vim, run: 

```bash
:set shell=/bin/bash
```

This will change the current shell to be `/bin/bash` instead.

Then run this to get a real bash shell:

```bash
:shell
```

Boom! Easy, we finally inside the server without any issue.

**Step 5: Retrieving the password**

I just run the following command:

```bash
cat /etc/bandit_pass/bandit26
```

Voila! A golden ticket for the next level has already been caught.

---

## The Password for the Next Level

`[REDACTED]`

## What I Learned

- Not all users are given a full shell like `/bin/bash`. Sometimes, it's replaced with a restricted command (like `more`) through scripts.
- The `exec` command in shell scripts **replaces** the current shell process - which can trap users in limited environemnts.
- Terminal window size can affect how programs behave. A large terminal may cause `more` to display the file and exit immediately, bypassing interactivity.
- Tools like `vim` have escape hatches that let you spawn a real shell, which can be used to break out of restricted environment.
- Always investigate the shell or startup command of a user when unexpected behaviour occurs during login.

## Helpful Reading Material

- [Linux exec Command](https://phoenixnap.com/kb/linux-exec#:~:text=The%20Linux%20exec%20command%20executes,different%20behaviors%20and%20use%20cases.)
- [Understanding /etc/passwd File Format](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)
- [Basic Vim](https://www.computerhope.com/unix/vim.htm)
- [More Command](https://www.tutorialspoint.com/unix_commands/more.htm)


   
