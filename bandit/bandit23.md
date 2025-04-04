# Bandit22 → 23: The Enigma of the Cron Script

[Challenge](https://overthewire.org/wargames/bandit/bandit23.html)

---

## Level Description

The password for the next level is hidden, guarded by a mysterious program running automatically via **cron**, the time-based jobs scheduler. My mission? To uncover the secrets of this cron job, decode its cryptic script, and retrieve the elusive password.

---

## The Process

**Step 1: Entering the Lair of bandit22**

I logged into the bandit22 server, the air thick with anticipation:

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

After entering the password, I found myself in an empty home directory. No files, no hints—just silence. But I knew better than to be fooled. The answer had to be lurking somewhere in the shadows of the system.

**Step 2: The Cron Job Clue**

The level description whispered of **cron job** running automatically. My instincts told me to investigate the `/etc/cron.d/` directory, where cron configurations are stored. I navigated there:

```bash
cd /etc/cron.d/
ls
```

The directory revealed several cron job files:

```bash
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
```

The file `cronjob_bandit23` caught my eye. It had to be the key to unlocking the next level.

**Step 3: Decoding the Cron Job**

I opened the `cronjob_bandit23` file to see what it was up to:

```bash
car cronjob_bandit23
```

The output was intriguing:

```bash
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

This meant that a script, `/usr/bin/cronjob_bandit23.sh`, was running every minute as the `bandit23` user. The `&> /dev/null` part ensured that any output was discarded, making it harder to detect. But I wasn’t going to let that stop me.

**Step 4: Unmasking the Script**

I decided to inspect the script to understand its purpose:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

The script contained the following:

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Here’s what it did:

1. It determined the current user (`whoami`).
2. It generated an **MD5 hash** of the string `"I am user <username>"` and stored it in the variable `mytarget`.
3. It copied the password for the current user (`/etc/bandit_pass/$myname`) to a file in `/file/` named after the MD5 hash.

This was a clever setup. The cron job was essentially leaking the password into a temporary file, but only for a brief moment.

**Step 5: Exploiting the Script**

Since the cron job runs as the `bandit23` user, I needed to figure out what the MD5 hash would be for `bandit23`. I ran the following command to generate the hash:

```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

The output was:

```bash
8ca319486bfbbc3663ea0fbe81326349
```

This meant that the password for Bandit23 was stored in **/tmp/8ca319486bfbbc3663ea0fbe81326349.**

**Step 6: Retrieving the Password**

I checked the contents of the file:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

And there it was—the password for bandit23!

---

## Password for the Next Level

`[REDACTED]`

## What I learned

- **Cron Jobs**: I learned how to inspect and understand cron job configurations in `/etc/cron.d/`.
- **Script Analysis**: This level reinforced the importance of reading and understanding scripts written by others.
- **MD5 Hashing**: I gained insight into how MD5 hashes can be used to generate unique filenames.

## Helpful Reading Material

- [Cron Jobs in Linux](https://linux.die.net/man/5/crontab): A guide to understanding cron jobs and their configuration.
- [MD5 Hashing](https://linux.die.net/man/1/md5sum): Documentation on generating MD5 hashes in Linux.
