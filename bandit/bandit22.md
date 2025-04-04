# Bandit21 → 22: The Secret of the Cron Job

[Challenge](https://overthewire.org/wargames/bandit/bandit22.html)

---

## Level Description

The password for the next level is hidden away, accessible only by the **`bandit22`** user. But there’s a twist: a program is running automatically at regular intervals via **cron**, the time-based job scheduler. My mission was to uncover the secrets of thic cron job and retrieve the password.

---

## The Process

**Step 1: Entering the Lair of bandit21**

I logged into bandit 21 server, feeling a mix of curiosity and determination:

```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
```

After entering the password, I found myself staring at an empty home directory. No files, no hints—just silence. But I knew better than to give up. The answer had to be somewhere in the system.

**Step 2: The Clue in the Shadows**

The level description mentioned **cron**, the time-based job scheduler. I had a lunch that the password was tied to a cron job running in the background. My first move was to investigate the **`/etc/cron.d/`** directory, where cron jobs are often configured.

I navigated there:

```bash
cd /etc/cron.d/
ls
```

The directory revealed several cron job files:

```bash
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
```

The file **`cronjob_bandit22`** caught my eye. It had to be the key.

**Step 3: Decoding the Cron Job**

I opened the **`cronjob_bandit22`** file to see what it was up to:

```bash
cat cronjob_bandit22
```

The output was intriguing:

```bash
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

This meat that a script, **`/usr/bin/cronjob_bandit22.sh`**, was running every minute as the `bandit22` user. The `&> /dev/null` part ensured that any output was discarded, making it harder to detect. But I wasn’t going to let that stop me.

**Step 4: Unmasking the Script**

I decided to inspect the script to understand its purpose:

```bash
cat /usr/bin/cronjob_bandit22.sh
```

The script contained the following:

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Here’s what it did:

1. It changed the permissions of a file in `/tmp/` to `644`, making it readable by everyone.
2. It wrote the password for bandit22 (`/etc/bandit_pass/bandit22`) into this file.

This was a clever setup. The cron job was essentially leaking the password into a temporary file, but only for a brief moment.

**Step 5: Seizing the Opportunity**

Since the cron job ran as the `bandit22` user, it had the necessary permissions to write the password to the temporary file. All I had to do was read the file before it was overwritten or deleted.

I checked the contents of the file:

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

And there it was—the password for bandit22.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Cron Jobs**: I learned how to inspect and understand cron job configurations in `/etc/cron.d/`.
- **File permissions:** This level reinforced of file permissions and how they can be exploited to access restricted data.
- **Automated Scripts**: I gained insight into how automated scripts can be used to perform tasks at regular intervals.

## Helpful Reading Material

- [Cron Jobs in Linux](https://linux.die.net/man/5/crontab): A guide to understanding cron jobs and their configuration.
- [File Permissions in Linux](https://linux.die.net/man/1/chmod): Documentation on file permissions and how to modify them.
