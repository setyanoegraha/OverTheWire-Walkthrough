# Bandit12 → 13: Peeling the Layers of Compression

[Challenge](https://overthewire.org/wargames/bandit/bandit13.html)

## Level Description

The password for the next level is stored in a file called `data.txt`. However, this file is not straightforward to read because it has been converted into a hexdump of a file that was compressed multiple times. Our task is to reverse this process and extract the password.

Since we will be dealing with multiple extractions, it’s a good idea to create a temporary working directory using `mktemp -d`. This prevents clutter and accidental file modifications. Then, we will copy the file into this new directory for safe analysis.

## The Process

I start by listing all the files in the home directory. 

```bash
bandit12@bandit:~$ ll -a
total 24
drwxr-xr-x  2 root     root     4096 Sep 19 07:08 ./
drwxr-xr-x 70 root     root     4096 Sep 19 07:09 ../
-rw-r--r--  1 root     root      220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root     root     3771 Mar 31  2024 .bashrc
-rw-r-----  1 bandit13 bandit12 2583 Sep 19 07:08 data.txt
-rw-r--r--  1 root     root      807 Mar 31  2024 .profile
```

like always one file that look interesting was named `data.txt`, therefore I checked on the type of its. 

```bash
bandit12@bandit:~$ file data.txt
data.txt: ASCII text
```

At first glance, it seems like a normal text file, so I try to read it.

I use the `cat` command to check the content. 

```bash
bandit12@bandit:~$ cat data.txt
00000000: 1f8b 0808 dfcd eb66 0203 6461 7461 322e  .......f..data2.
00000010: 6269 6e00 013e 02c1 fd42 5a68 3931 4159  bin..>...BZh91AY
00000020: 2653 59ca 83b2 c100 0017 7fff dff3 f4a7  &SY.............
00000030: fc9f fefe f2f3 cffe f5ff ffdd bf7e 5bfe  .............~[.
00000040: faff dfbe 97aa 6fff f0de edf7 b001 3b56  ......o.......;V
00000050: 0400 0034 d000 0000 0069 a1a1 a000 0343  ...4.....i.....C
00000060: 4686 4341 a680 068d 1a69 a0d0 0068 d1a0  F.CA.....i...h..
00000070: 1906 1193 0433 5193 d4c6 5103 4646 9a34  .....3Q...Q.FF.4
00000080: 0000 d320 0680 0003 264d 0346 8683 d21a  ... ....&M.F....
00000090: 0686 8064 3400 0189 a683 4fd5 0190 001e  ...d4.....O.....
000000a0: 9034 d188 0343 0e9a 0c40 69a0 0626 4686  .4...C...@i..&F.
000000b0: 8340 0310 d340 3469 a680 6800 0006 8d0d  .@...@4i..h.....
000000c0: 0068 0608 0d1a 64d3 469a 1a68 c9a6 8030  .h....d.F..h...0
000000d0: 9a68 6801 8101 3204 012a ca60 51e8 1cac  .hh...2..*.`Q...
000000e0: 532f 0b84 d4d0 5db8 4e88 e127 2921 4c8e  S/....].N..')!L.
000000f0: b8e6 084c e5db 0835 ff85 4ffc 115a 0d0c  ...L...5..O..Z..
00000100: c33d 6714 0121 5762 5e0c dbf1 aef9 b6a7  .=g..!Wb^.......
00000110: 23a6 1d7b 0e06 4214 01dd d539 af76 f0b4  #..{..B....9.v..
00000120: a22f 744a b61f a393 3c06 4e98 376f dc23  ./tJ....<.N.7o.#
00000130: 45b1 5f23 0d8f 640b 3534 de29 4195 a7c6  E._#..d.54.)A...
00000140: de0c 744f d408 4a51 dad3 e208 189b 0823  ..tO..JQ.......#
00000150: 9fcc 9c81 e58c 9461 9dae ce4a 4284 1706  .......a...JB...
00000160: 61a3 7f7d 1336 8322 cd59 e2b5 9f51 8d99  a..}.6.".Y...Q..
00000170: c300 2a9d dd30 68f4 f9f6 7db6 93ea ed9a  ..*..0h...}.....
00000180: dd7c 891a 1221 0926 97ea 6e05 9522 91f1  .|...!.&..n.."..
00000190: 7bd3 0ba4 4719 6f37 0c36 0f61 02ae dea9  {...G.o7.6.a....
000001a0: b52f fc46 9792 3898 b953 36c4 c247 ceb1  ./.F..8..S6..G..
000001b0: 8a53 379f 4831 52a3 41e9 fa26 9d6c 28f4  .S7.H1R.A..&.l(.
000001c0: 24ea e394 651d cb5c a96c d505 d986 da22  $...e..\.l....."
000001d0: 47f4 d58b 589d 567a 920b 858e a95c 63c1  G...X.Vz.....\c.
000001e0: 2509 612c 5364 8e7d 2402 808e 9b60 02b4  %.a,Sd.}$....`..
000001f0: 13c7 be0a 1ae3 1400 4796 4370 efc0 9b43  ........G.Cp...C
00000200: a4cb 882a 4aae 4b81 abf7 1c14 67f7 8a34  ...*J.K.....g..4
00000210: 0867 e5b6 1df6 b0e8 8023 6d1c 416a 28d0  .g.......#m.Aj(.
00000220: c460 1604 bba3 2e52 297d 8788 4e30 e1f9  .`.....R)}..N0..
00000230: 2646 8f5d 3062 2628 c94e 904b 6754 3891  &F.]0b&(.N.KgT8.
00000240: 421f 4a9f 9feb 2ec9 83e2 c20f fc5d c914  B.J..........]..
00000250: e142 432a 0ecb 0459 1b15 923e 0200 00    .BC*...Y...>...
```

Instead of readable text, I see what looks like a hex dump or compressed data. This suggests that the file is encoded or compressed.

Since I’ll be working with multiple extracted files, I create a temporary directory using `mktemp -d` and move into it. 

```bash
bandit12@bandit:~$ mktemp -d
/tmp/tmp.HNxlyytlFS
bandit12@bandit:~$ cd /tmp/tmp.HNxlyytlFS
```

This keeps my workspace clean.

I copy `data.txt` into the temporary directory and rename it to `data.bin`.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ cp ~/data.txt .
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ ls
data.txt
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data.txt data.bin
```

Since the file appears to be in a hex dump format, I use `xxd -r` convert it back to binary.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ xxd -r data.txt > data.bin
```

Now, let's check the file type again.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ file data.bin
data.bin: gzip compressed data
```

It is a `gzip` file, so we rename it and decompress it.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data.bin data.gz
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ gunzip data.gz
```

Now, we check the new file.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ file data
data: bzip2 compressed data
```

---

The file is now in `bzip2` format, so we rename and decompress it.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data data.bz2
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ bunzip2 data.bz2
```

Checking the file type again:

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ file data
data: gzip compressed data
```

We repeat the decompression process.

```
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data data.gz
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ gunzip data.gz
```

Checking the file:

```
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ file data
data: POSIX tar archiv
```

---

The file is now a `.tar` archive, so we extract it.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data data.tar
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ tar -xvf data.tar
```

This reveals a new file, `data5.bin`, which is another `.tar` archive.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data5.bin data5.tar
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ tar -xvf data5.tar
```

This extracts `data6.bin`, which is compressed with `bzip2`.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data6.bin data6.bz2
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ bunzip2 data6.bz2
```

Checking the file:

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ file data6
data6: POSIX tar archive
```

We extract it again.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data6 data6.tar
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ tar -xvf data6.tar
```

This gives us `data8.bin`, which is `gzip` compressed.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ mv data8.bin data8.gz
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ gunzip data8.gz
```

Checking the final file:

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ file data8
data8: ASCII text
```

Finally, A peak of the file already found, `cat` the file and we got the password.

```bash
bandit12@bandit:/tmp/tmp.HNxlyytlFS$ cat data8
The password is [REDACTED]
```

Voila! A long journey to go And finally, a beautiful password became ours!

## Password for the Next Level

`[REDACTED]`

## What I Learned

- How to work with **hex dumps** and convert them back into binary using `xxd -r`.
- The importance of using `file` to identify file types at each stage of extraction.
- How to handle **multiple ocmpression formats,**  including `gzip`, `bzip2`, and `tar`.
- The benefits of  using `mktemp -d` to create a temporary working directoyr for clean and organized extraction.
- The iterative process of **renaming and extracting** files based on their type.
- How data can be **layered with multiple compression** to obscure information.
- Gained **patience and problem-solving skills**  when dealing with recursive decompression challenges.

## Helpful Reading Material

- [File Command in Linux](https://www.geeksforgeeks.org/file-command-in-linux-with-examples/)
- [Linux File Command](https://phoenixnap.com/kb/linux-file-command)
- [About mktemp](https://www.tutorialspoint.com/unix_commands/mktemp.html)
- [How to use gzip and bzip2 in Linux](https://www.computernetworkingnotes.com/linux-tutorials/how-to-use-gzip-and-bzip2-linux-commands-explained.html)
- [Tar Command](https://www.geeksforgeeks.org/tar-command-linux-examples/)
- [Command Line for Beginners](https://ubuntu.com/tutorials/command-line-for-beginners)
- [XXD Command](https://linuxhandbook.com/xxd-command/)
- [XXD](https://www.tutorialspoint.com/unix_commands/xxd.html)
