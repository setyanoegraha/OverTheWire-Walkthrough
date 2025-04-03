# Bandit06 → 07: Finding the Right File

[Challenge](https://overthewire.org/wargames/bandit/bandit7.html)

## Level Description

The goal for this level was to find the password stored in a file somewhere on the server, which must meet the following criteria:

- Owned by user `bandit7`
- Owned by group `bandit6`
- Exactly 33 bytes in size.

## The Process

After logging into the server as `bandit6`, I started by listing the contents of the current directory with `ll -a`, but I didn’t find anything useful. Since the password file could be located anywhere on the system, I used the `find` command to search for files that met the specified criteria.

I ran the following command:

```bash
$ find / -type f -size 33c -user bandit7 -group bandit6
```

This command searched for files:

- `-type f` specifies files only,
- `-size 33c` limits the results to files that are exactly 33 bytes in size,
- `-user bandit7` and `-group bandit6` ensure the file is owned by the correct user and group

This command did return a lof of permission denied messages for directories I didn’t have access to.

```bash
find: ‘/drifter/drifter14_src/axTLS’: Permission denied
find: ‘/root’: Permission denied
find: ‘/snap’: Permission denied
find: ‘/tmp’: Permission denied
find: ‘/proc/tty/driver’: Permission denied
find: ‘/proc/2002197/task/2002197/fdinfo/6’: No such file or directory
find: ‘/proc/2002197/fdinfo/5’: No such file or directory
find: ‘/home/bandit31-git’: Permission denied
find: ‘/home/ubuntu’: Permission denied
find: ‘/home/bandit5/inhere’: Permission denied
find: ‘/home/bandit30-git’: Permission denied
find: ‘/home/drifter8/chroot’: Permission denied
find: ‘/home/drifter6/data’: Permission denied
find: ‘/home/bandit29-git’: Permission denied
find: ‘/home/bandit28-git’: Permission denied
find: ‘/home/bandit27-git’: Permission denied
find: ‘/lost+found’: Permission denied
find: ‘/etc/polkit-1/rules.d’: Permission denied
find: ‘/etc/multipath’: Permission denied
find: ‘/etc/stunnel’: Permission denied
find: ‘/etc/xinetd.d’: Permission denied
find: ‘/etc/credstore.encrypted’: Permission denied
find: ‘/etc/ssl/private’: Permission denied
find: ‘/etc/sudoers.d’: Permission denied
find: ‘/etc/credstore’: Permission denied
find: ‘/dev/shm’: Permission denied
```

And so on. But eventually, it returned the file `/var/lib/dpkg/info/bandit7.password` 

Unfortunately, the output is not very readable and one has to look carefully to find the file address. Therefore, I used this command instead:

```bash
$ find / -type f -size 33c -user bandit7 -group bandit6 -exec file {} \; 2>/dev/null
```

As you can see there are additional commands:

- `-exec file {} \;` For each file found, run the `file` command on it to determine its type.
- `2>/dev/null` Redirect **error messages** (like permission denied) to `/dev/null`, so they don’t clutter the output.

This command return:

```bash
/var/lib/dpkg/info/bandit7.password: ASCII text
```

Finally, I used the `cat` to read the contents of the file:

```bash
$ cat /var/lib/dpkg/info/bandit7.password
```

The password for the next level appeared!

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Using `find` with Multiple Criteria:** The `find` commands is highly flexible, allowing me to combine multiple filters like file size, ownership, and permissions to find exactly what I needed.
- **Handling Permission Denied Errors:** When searching in directories without access, it’s important to suppress errors using `2>/dev/null` to keep the output clean.
- **File Verification with `file` :** Using `file` on found files confirmed that the file was indeed human-readable, and the content was what I expected.

## Helpful Reading Material

- [Find Command in Linux](https://www.geeksforgeeks.org/find-command-in-linux-with-examples/)
- [Linux Error Redirection](https://www.geeksforgeeks.org/linux-error-redirection/)
