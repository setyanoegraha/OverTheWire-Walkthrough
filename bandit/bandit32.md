# Bandit31 → 32: Git It Pushed!

[Challenge](https://overthewire.org/wargames/bandit/bandit32.html)

---

## Level Description

In this level, the challenge shifts from pulling secrets to **contributing** them. You're not jsut reading files anymore; you're writing them to a remote Git repository. The task is to **push a file named** `key.txt` **with the content** `May I come in?` **to the master branch** of a remote Git repo. Success means you've not only mastered cloning, but also pushing your way through!

---

## The Process

As I logged into `bandit31`, I knew this wasn't going to be a simple `cat` and `grep` kind of level. It was time to take what I knew about Git and apply it with a bit more finesse.

To start, I created a clean working environment. I typed:

```bash
mktemp -d
```

The system gave me something like `/tmp/tmp.ymI7tZiklj`. I navigated into that directory:

```bash
cd /tmp/tmp.ymI7Ziklj
```

Once inside, I cloned the Git repository provided in the level:

```bash
git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
```

Git warned me about the authenticity of the host. I confidently replied `yes`. This OverTheWire, after all. I was then asked for the password, which I still had from level 31.

The repository cloned successfully. I stepped into it:

```bash
cd repo
```

A quick `ls` showed me a single file: `README.md`. Naturally, I opened it:

```bash
cat README.md
```

It spelled out the mission clearly:

```bash
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

Alright. Nothing tricky. I created the file with the specified content:

```bash
echo "May I come in?" > key.txt
```

Double-check:

```bash
cat key.txt
```

Perfect.

Now, Git has a way of ignoring certain files via `.gitignore`, so I made sure to **force add** this one:

```bash
git add -f key.txt
```

Next, I committed it. Git tried to open `nano`, which complained a bit about missing folders, but I didn't let that stop me:

```bash
git commit -a
```

I typed a commit message like "First Commit" and saved.

Then came the final push:

```bash
git push origin master
```

Once again, Git prompted me about authencity; and once again, I said `yes` and entered the password.

The server thought for a moment... and then it hit me with the success message:

```bash
Well done! Here is the password for the next level:
```

Mission accomplished.

---

## Password for the Next Level

`[REDACTED]`

---

## What I Learned

This level reinforced how Git operates with **remote repositories**, especially the `push` workflow. I also got practice:

- Cloning a repo from an SSH source
- Adding files to a repo (even those ignored by default)
- Committing changes without relying on Git configs
- Using `git push` with authentication and SSH host verification

More importantly, it flipped the usual script: instead of extracting information, I had to **submit** it correctly; a useful skill for version control collaboration in real-life projects.

---

## Helpful Reading Material

- [Git Add](https://git-scm.com/docs/git-add)
- [Git Commit](https://git-scm.com/docs/git-commit)
- [Git Push](https://git-scm.com/docs/git-push)
- [.gitignore](https://git-scm.com/docs/gitignore)
- [What is gitignore and How to use it?](https://www.geeksforgeeks.org/what-is-git-ignore-and-how-to-use-it/)

