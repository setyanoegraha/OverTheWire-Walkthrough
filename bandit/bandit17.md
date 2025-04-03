# Bandit16 → 17: Navigating Ports and Keys

[Challenge](https://overthewire.org/wargames/bandit/bandit17.html)

## Level Description

The description for the next level can be retrieved by submitting the password of the current level to a port on **`localhost` in the range** `31000` to **`32000`**. First, I needed to find out which of these ports have a server listening on them. Then, I had to determine which of those speak SSL/TSL and which don’t. There is only **one server** that will provide the next credentials.

## The Process

**Step 1: Connecting to Bandit16**

I started by connecting to the Bandit16 server using SSH:

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

After entering the password from the previous level, I was in.

**Step 2: Scanning for Open Ports**

The challenge hinted that the password was hidden behind one of the ports in the range 31000-32000. I used **`nmap`** to scan these ports:

```bash
nmap localhost -p 31000-32000
```

The scan revealed several open ports:

```bash
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

**Step 3: Identifying the Port**

To narrow down the list, I ran a service version detection scan:

```bash
nmap -sV localhost -p 31000-32000
```

This showed that **port 31790** was running an SSL service, making it the most likely candidate.

**Step 4: Connecting Using SSL**

I used **`openssl`** to connect to port 31790:

```bash
openssl s_client -ign_eof localhost:31790
```

After establishing the connection, the server prompted me for the current password. I entered the password for Bandit16, and it responded with:

```bash
Correct!
-----BEGIN RSA PRIVATE KEY-----
[Private Key Content]
-----END RSA PRIVATE KEY-----
```

The server provided an RSA private key, which I needed to use to log in as Bandit17.

**Step 5: Saving the Private Key**

I created a temporary directory to store the private key securely:

```bash
mktemp -d
```

This returned **`/tmp/tmp.W40VPDbIBS`**, so I navigated there:

```bash
cd /tmp/tmp.W4OVPDbIBS
```

I opened a new file called **`sshkey.private`** using **`vim`** and pasted the private key content to it.

**Step 6: Fixing Permissions**

When I tried to use the private key to SSH into Bnadit17, I encountered an error:

```bash
Permissions 0664 for 'sshkey.private' are too open.
```

SSH requires private keys to have strict permissions. I fixed this by running:

```bash
chmod 700 sshkey.private
```

**Step 7: Logging into Bandit17**

With the permissions fixed, I attempted to log in again:

```bash
ssh -i sshkey.private bandit17@localhost -p 2220
```

This time, it worked! I was greeted with the Bandit17 shell.

**Step 8: Retrieving the Password**

Finally, I retrieved the password for Bandit17:

```bash
cat /etc/bandit_pass/bandit17
```

The password was:

```bash
[REDACTED]
```

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Port Scanning:**  I learned how to use **`nmap`** to scan for open ports and identify services running on them.
- **SSL Connections:** I gained experience using **`openssl`** to establish secure connections and retrieve data.
- **SSH Key Management:** I understood the importance of file permissions for SSH private keys and how to set them correctly.
- **Troubleshooting:** I improved my ability to diagnose and fix errors, such as permission issues.

## Helpful Reading Material

- [Nmap Documentation](https://nmap.org/book/man.html): A comprehensive guide to using `nmap` for network exploration.
- [OpenSSL Documentation](https://www.openssl.org/docs/): Official documentation for using `openssl` to establish secure connections.
- [SSH Key Permissions](https://www.ssh.com/academy/ssh/key): A guide to managing SSH key permissions securely.
