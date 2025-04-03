# Bandit13 → 14: A Private Key Adventure

[Challenge](https://overthewire.org/wargames/bandit/bandit14.html)

## Level Description

You’ve reached Bandit Level 13, and this time, things take an interesting turn. Instead of being given the next password directly, you’re handed an SSH private key. The goal? Use this key to log in as `bandit14` and retrieve the password stored in `/etc/bandit_pass/bandit14`. Sounds like a mission!

## The Process

As always, the journey begins by loggin into the Bandit server as `bandit13.` Here’s how it plays out:

1. **Connect to the Bandit server:**
    
    ```bash
    ssh bandit13@bandit.labs.overthewire.org -p 2220
    ```
    
    Once inside, take a look around.
    
2. **Check what’s available in the home directory:**
    
    ```bash
    ls
    ```
    
    The output reveals a file named `sshkey.private`. That’s our golden ticket!
    
3. **Use the SSH key to log in as `bandit14`:**
    
    ```bash
    ssh -i sshkey.private bandit14@localhost -p 2220
    ```
    
    If a security prompt appears asking if you trust the connection, type `yes` and press Enter.
    
4. **Retrieve the password:**
    
    ```bash
    cat /etc/bandit_pass/bandit14
    ```
    
    Output:
    
    ```bash
   [REDACTED] 
    ```
    
    Voila! Mission accomplished!
    

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Using an SSH private key for authentication:** Instead of a password, private keys provide a secure way to log in.
- **The concept of `localhost`:** This refers to the same machine you’re currently logged into, useful for local SSH connections.
- **How `cat` helps read protected files**: The `/etc/bandit_pass/` directory contains passwords, but access is only allowed to the correct user.
- **Why private keys should be kept secure**: Since the SSH key allowed authentication without a password, it highlights the importance of keeping private keys protected.

## Helpful Reading Material

- `man ssh`
- [Keys of OpenSSH](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)
- [SSH to remote server using a private key](https://unix.stackexchange.com/questions/23291/how-to-ssh-to-remote-server-using-a-private-key)
