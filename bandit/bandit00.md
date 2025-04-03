# bandit00: Log Into Remote Server

[Bandit 0](https://overthewire.org/wargames/bandit/bandit0.html)

## Level description

This was the starting point of the bandit wargame. The goal seemed straightforward: log into a remote server using SSH. The host was [`bandit.labs.overthewire.org`](http://bandit.labs.overthewire.org), port `2220`. They even gave me the username and password—both were `bandit0`. Sounds simple enough, right? Let’s dive in.

## The Process

So there I was, sitting at my terminal, ready to conquer the first challenge. The instructions were clear: I needed to use SSH, but there was one small twist—port `2220`. SSH, by default, connects on port `22`, so I knew I had to explicitly specify the port this time.

Here’s how I did it:

I fired up my terminal, typed the command:

```bash
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
```

When prompted for the password, I entered `[REDACTED]`.

Boom! I was in. I saw the welcome message from Bandit’s server, which told me I had successfully logged in. It felt like the first small victory in a long adventure.

## Password for the Next Level

`[REDACTED]`

## What I learned

- **Using SSH:** I got hands-on experience connecting to a remote server.
- **Specifying Ports:** By default, SSH uses port 22, but for this challenge, I learned how to connect to a custom port with the `-p` flag.
- **Basic Authentication:** It’s always good to know how to log in with just a username and password.

## Helpful Reading Material

- Always check out `man ssh` on your terminal—It’s like having the SSH manual right there in your pocket.
- If you’re new to SSH, check out this guide: [OpenSSH Server Documentation](https://ubuntu.com/server/docs/openssh-server)
