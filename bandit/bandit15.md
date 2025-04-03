# Bandit14 → 15: Talking to a Port

[Challenge](https://overthewire.org/wargames/bandit/bandit15.html)

## Level Description

This time, instead of dealing with files, we need to communicate with a specific port on `localhost`. The goal is to find the password for `bandit15` by connecting to the correct port and reading the output. Sounds like an interesting challenge!

## The Process

1. **Logging in as `bandit14`**
    
    I started by logging into the server with the credentials form the previous level
    
    ```bash
    ssh bandit14@bandit.labs.overthewire.org -p 2220
    ```
    
    Once logged in, I was ready to begin my investigation.
    
2. **Checking for Clues**
    
    Running `ls` in the home directory revealed no files. This meant the solution wasn’t about reading a file but likely involved interacting with the system differently. Since the challenge description mentioned a port, I suspected this was a networking challenge.
    
3. **Exploring Useful Tools**
    
    Without any files to inspect, I needed to figure out how to interact with a port. To check what networking tools might be available, I ran:
    
    ```bash
    whatis nc
    whatis telnet
    whatis openssl
    ```
    
    The descriptions confirmed that these tools deal with network communication. I now had some possible methods to proceed.
    
4. **First Attempt: Using `nc` (Netcat)**
    
    My first thought was to use `nc`, a tool often used for connecting to and listening on network ports. I attempted:
    
    ```bash
    nc -p 30000
    ```
    
    However, this resulted in an error. After reviewing the manual, I realized that `-p` in `nc` specifies the **source** port rather than the **destination port**. This meant I was using the wrong flag and needed a different approach.
    
5. **Understanding `telnet` vs `nc`**
    
    At this point, I paused to understand the difference between `nc` and `telnet`:
    
    - `nc` **(netcat)** is a flexible networking tool for sending and receiving raw data over TCP/UDP. It requires precise options and is often used for scripting or penetration testing.
    - `telnet` is a simpler tool meant for interacting with text-based services over a network. Unike `nc`, it assumes the fiven number is **the destination port**, making it more user-friendly for this challenge.
    - **Why `-p` in `nc` but not in `telnet`?**
        - In `nc`, `-p` sets the **source** port, which is unnecessary when connecting to a service.
        - `telnet` assumes the number provided is **destination** port, making it the better choice here.
6. **Using `telnet` to Connect**
    
    Armed with this understanding, I attempted:
    
    ```bash
    telnet localhost 30000
    ```
    
    Succes! The connection was established, and the service on port `30000` responed with:
    
    ```bash
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.
    ```
    
    At first, I was confused as to why there was a blank space on it. It turns out the connections waited for the password to pass, so I just sent the current password and pressed Enter.
    
    ```bash
    [REDACTED]
    Correct!
    [REDACTED] 
    
    Connection closed by foreign host.
    ```
    
    The first string was the password for the current level, but the second one looked like a password for the next level! Voila! Beautiful password appear!
    

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **How to connect to a remote service using `telnet`,** which is useful for testing open ports and communicating with network services.
- **The difference between `nc` and `telnet`**
    - `nc` allows for flexible raw data transfer but requires precise flags.
    - `telnet` is more straightforward for human-readable communication.
- **Why `nc` requires `-p` but `telnet` does not:**
    - `nc -p` is used to specify a **source** port, which isn’t needed for simple connections
    - `telnet` assumes the **destination** port automatically, making it more intuitive for this challenge.
- **How `localhost` works,** referring to the local machine, allowing interaction with services running internally.
- **How to extract information from a listening port**, demonstrating how services on specific ports can yield useful data if accessed correctly.

## Helpful Reading Material

- `man telnet`
- `man nc`
- `man openssl`
- [Telnet Netcat Troubleshooting](https://www.redhat.com/en/blog/telnet-netcat-troubleshooting)
- [NC Linux Command](https://ioflood.com/blog/nc-linux-command/)
- [Web Server](https://computer.howstuffworks.com/web-server8.htm)
- [What is Localhost](https://www.geeksforgeeks.org/what-is-local-host/)
