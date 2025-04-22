# Bandit30 → 31: Secrets Hidden in Tags

[Challenge](https://overthewire.org/wargames/bandit/bandit31.html)

---

## Level Description

In this level, we're once again dealing with Git, but things feel sneakier this time. The username is `bandit30` and we need to connect to a private repository hosted locally on the machine via SSH. According to the level description, our goal is retrieve the password for the next level from a remote Git repository. But this time, there's no file with a suspicious name or a bunch of branches-just a simple `README.md`. Could it really be that simple?

---

## The Process

After logging in via SSH:

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
```

I created a safe temporary working directory using:

```bash
mktemp -d
cd /tmp/tmp.oWYcMr58qB
```

Then I cloned the repository:

```bash
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
```

There was a brief prompt asking to trust the host. I typed `yes` and entered the password provided from the previous level. The repo was cloned succesfully.

When I listed the contents:

```bash
ls repo
```

It only contained a single file: `README.md`. The file, upon inspection, seemed like a red herring:

```bash:
cat README.md
```

It cheekily read:
`just an epmty file... muahaha`
Hmm. Classic misdirection.

Next, I checked out the branches:

```bash
git branch -a
```

Only `master` and a remote-tracking branch were visible. Nothing new there. But remembering how Git can also hide goodies in *tags*, I listed the tags:

```bash
git tag
```

Boom. A single tag appeared:
`secret`

Now I was curious. I used:

```bash
git show secret
```

And there it was-hidden in plain sight inside a Git tag; That's the password.

---

## Password for the Next Level

`[REDACTED]`

--- 

## What I Learned

This level is a great reminder that Git isn't just about commits and branches. Tags can be used to mark important points in history, and yes-hide secrets. It taught me to always check *every* part of a Git repository: branches, commits, tags, stashes, and so on. The `git show` command in particular, is handy for revealing the content associated with a tag or commit.

Also, it reinforced the value of misdirection in Wargames-style challenges. Just because a `README.md` says there's nothing there doesn't mean the repository has nothing!

---

## Helpful Reading Material

- [Git Tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
- [Git Show](https://git-scm.com/docs/git-show)
- [Git Branch](https://git-scm.com/docs/git-branch)

