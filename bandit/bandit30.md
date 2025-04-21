# Bandit29 → 30: Hidden in Branches

[Challenge](https://overthewire.org/wargames/bandit/bandit30.html)

---

## Level Description

We've made it to bandit29, and once again, we're dealing with Git. This time, the level description is short and sweet: clone a Git repo and find the password. No hints, no riddles-just raw exploration. But I've learned by now that Git likes to hide secrets in its shadows: commits, branches, and maybe even tags. Time to get out hands dirty.

---

## The Process

Just like in the previous level, I kicked things off by creating a temporary directory to work in:

```bash
mktemp -d
```

This gave me a new directory, which I jumped into:

```bash
cd /tmp/tmp.USURD5v8wK
```

Then, I cloned the repo using the provided SSH address and the usual prot `2220`:

```bash
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
```

I typed `yes` to accept the server fingerprint (again), then entered the same password used to log in as `bandit29`. After cloning, I entered the repository:

```bash
cd repo
```

At first glance, the only file there was `README.md`. I opened it up with:

```bash
cat README.md
```

It looked promising-until I saw:

```diff
- username: bandit30
- password: <no passwords in production!>
```

No password. Just a placeholder. But I wasn't surprised.

Knowing that secrets might be lurking in Git branches, I ran:

```bash
git branch -a
```

That revealed a few omre paths to explore:

```bash
* master
  remotes/origin/dev
  remotes/origin/sploits-dev
```

The `master` branch was already checked out and had nothing useful, so I turned to `dev`. Maybe the developer left something behind. I switched to that branch:

```bash
git checkout dev
```

Then I looked at the commit history with:

```bash
git log
```

There were a couple of commits here, and that was a good sign. I decided to go back and read the `README.md` file again-this time on the `dev` branch:

```bash
cat README.md
```

And there it was-clear as day.

---

## Password for the Next Level

`[REDACTED]`

---

## What I Learned

This level reinforced an important lesson: **always check the branches**. Developer often switch between branches when building features or experimenting, and sometimes sensitive information is accidentally committed along the way. Even if it's **"cleaned up"** in the `master` branch, that doesn't mean it's gone from the repo entirely. 
Also, this level deepened my understanding of how Git branches work. I learned that even if two branches contain a file with the same name-like `README.md` in this challenge-the content inside can be completely different. That's because each branch in Git maintains its own version of every file. When switching branches using `git checkout`, Git replaces the working directory's files with the versions from the target branch.
This helped me understand Git as more than just a version control tool-it's like managing alternate timelines for your project, where files may look the same on the outside but carry completely different information inside depending on the branch. It also taught me the value of checking all branches when searching for hidden information in a repository.

---

## Helpful Reading Material

- [Git Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Git Checkout](https://git-scm.com/docs/git-checkout)
- [Git Log](https://git-scm.com/docs/git-log) 
