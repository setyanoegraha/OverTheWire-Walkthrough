# Bandit28 → 29: Git History Never Forgets

[Challenge](https://overthewire.org/wargames/bandit/bandit29.html)

---

## Level Description

We've reached bandit28, and this time we're dealing with a remote Git repository. The challenge is to clone a repo using SSH and dig around to uncover the password for the next level. The clue here is that the password used to be in the repository, buy may have been removed. So, it's up to us to dig through the Git history and retrieve what was once hidden.

---

## The Process

I Started on a clean slate by creating a temporary directory using:

```bash
mktemp -d
```

This gave me a workspace under `/tmp`, so I moved into it and prepared to clone the repository.

```bash
cd /tmp/tmp.Ap3czh0WXI
```

I Then cloned the Git repository using the provided SSH address and port:

```bash
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
```

Since this is an SSH connection, I was prompted to trust the host. I typed `yes` to continue and entered the same password used to log in as `bandit28`.

Once inside the cloned repo, I listed its contents:

```bash
cd repo
ls -la
```

There was a `README.md` file, so naturally, I read it:

```bash
cat README.md
```

It contained credentials

```bash
- username: bandit29
- password: xxxxxxxxxx
```

But the password had clearly been redacted. Suspicious.

So I peeked inside the `.git` directory and ran:

```bash
git log
```

Three commits appeared in the history. One of them, titled **"fix info leak"**, definitely sounded like the commit that censored the password.

To investigate what was changed, I ran:

```bash
git show 817e303aa6c2b207ea043c7bba1bb7575dc4ea73
```

This commit replaced the real password with `xxxxxxxxxx`. That meant the password existed in a previous commit. I looked at the one right before it:

```bash
git show 3621de89d8eac9d3b64302bfb2dc67e9a566decd
```

And there it was, hidden in plain sight, That's the one.

---

## Password for the Next Level

`[REDACTED]`

---

## What I Learned

This level taught me that Git repository is never truly private unless srubbed properly. Even if you "fix" a leak by editing a file and pushing the change, the original commit is still here, preserved like a fossil for anyone to inspect.

It also gave hands-on experience working with Git over SSH, exploring commit logs, and viewing diffs. This is incredibly useful for both devs and security professionals like.

---

## Helpful Reading Material

- [Git Basic - Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
- [Git Show](https://git-scm.com/docs/git-show)
- [Git Log](https://git-scm.com/docs/git-log)
