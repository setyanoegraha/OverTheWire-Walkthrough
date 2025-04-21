# Bandit27 → 28: Following the Git Trail of Secrets

[Challenge](https://overthewire.org/wargames/bandit/bandit28.html)

---

## Level Description

In this level, I was told that a Git repository existed at:

```bash
ssh://bandit27-git@localhost/home/bandit27-git/repo
```

It could be accessed over SSH on port 2220, and I needed to use the same password I had form the previous level. The goal? **Clone that repository and dig around**, somewhere inside, the password for the next level was waiting to be uncovered.

---

## The Process

As always, I began by setting up a clean and isolated workspace. I used `mktemp` to create a temporary directory and moved into it:

```bash
mktemp -d
cd /tmp/tmp.w4xZyEDWhr
```

Once I was inside, I initiated the cloning process:

```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```

As expected, Git asked me to verify the SSH key of the remote server. I typed `yes` to continue.
Though there were warnings about being unable to write to `/home/bandit27/.ssh/known_hosts` due to permission issues, I wasn't too concerned, There were just warnings and didn't prevent the cloning process.

The came the password prompt for `bandit27-git`. I entered the **same password from bandit27**, and just like that, Git started pulling in the repository files.

When the cloning finished, I navigated into the new `repo` directory:

```bash
cd repo
```

Inside, there was only one file-`README`. A file this prominent always feels like it's holding something important, so naturally, I opened it:

```bash
cat README
```

There it was! No tricks. No traps. Just a simple message hiding in plain sight. Sometimes, the hardest part is just knowing where to look.

---

## Password for the Next Level

`[REDACTED]`

---

## What I Learned

- How to clone a repository using SSH on a custom port.
- Dealing with SSH fingerprint prompts and permission warnings.
- The importance of exploring even the most innocent-looking files.

---

## Helpful Reading Material

- [Git Clone (Official Docs)](https://git-scm.com/docs/git-clone)
