# Leviathan04 -> 05

Challenge: `leviathan4@leviathan.labs.overthewire.org -p 2223`

## Level Description

locate the password for the next level by decode a binary named `bin` file on `~/.trash` directory.

## The Process

As usual, I started by checking the home directory and noticed a hidden folder named `.trash`:
```bash
leviathan4@gibson:~$ ll -a
total 24
drwxr-xr-x  3 root root       4096 Apr 10 14:23 ./
drwxr-xr-x 83 root root       4096 Apr 10 14:24 ../
-rw-r--r--  1 root root        220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root root       3771 Mar 31  2024 .bashrc
-rw-r--r--  1 root root        807 Mar 31  2024 .profile
dr-xr-x---  2 root leviathan4 4096 Apr 10 14:23 .trash/
```
I navigated into the `.trash` directory and found a single binary file called `bin`:
```bash
leviathan4@gibson:~$ cd .trash/
leviathan4@gibson:~/.trash$ ll -a
total 24
dr-xr-x--- 2 root       leviathan4  4096 Apr 10 14:23 ./
drwxr-xr-x 3 root       root        4096 Apr 10 14:23 ../
-r-sr-x--- 1 leviathan5 leviathan4 14940 Apr 10 14:23 bin*
```
This file is another **setuid binary**, this time owned by `leviathan5`, which means running it might give us elevated access again.

I executed the binary and it printed a string of binary numbers:
```bash
leviathan4@gibson:~/.trash$ ./bin
00110000 01100100 01111001 01111000 01010100 00110111 01000110 00110100 01010001 01000100 00001010
```

Clearly, this is binary-encoded text. To decode it, I wrote a short Python script:
```python
binary_data = "00110000 01100100 01111001 01111000 01010100 00110111 01000110 00110100 01010001 01000100 00001010"

binary_values = binary_data.split()

ascii_chars = [chr(int(b, 2)) for b in binary_values]

decoded_message = ''.join(ascii_chars)
print(f"Decoded: {decoded_message} ")
```
Running the script revealed the password for the next level — a sweet moment indeed!

## Conclusion

This level required decoding binary data output from a setuid binary hidden inside a `.trash` directory. Upon executing the file, it printed a series of binary values that represented ASCII characters. By writing a simple Python script to convert these binary strings to readable text, I was able to extract the password for the next level.

## Helpful Reading Material

- [Binary to ASCII Conversion](https://www.geeksforgeeks.org/program-binary-decimal-conversion/)
- [chr() in Python](https://docs.python.org/3/library/functions.html#chr)
- [int() in Python](https://docs.python.org/3/library/functions.html#int)
- [Hidden Files and Directories in Linux](https://phoenixnap.com/kb/show-hidden-files-linux)
