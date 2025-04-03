# Bandit11 → 12: Caesar 13

[Challenge](https://overthewire.org/wargames/bandit/bandit12.html)

## Level Description

In this level, the password we are looking for is stored in the file `data.txt`, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

## The Process

Upon logging in as `bandit11`, I Always check the contents of home directory and it’s file type if there it is. I just do the following:

```bash
bandit11@bandit:~$ ll -a
total 24
drwxr-xr-x  2 root     root     4096 Sep 19 07:08 ./
drwxr-xr-x 70 root     root     4096 Sep 19 07:09 ../
-rw-r--r--  1 root     root      220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root     root     3771 Mar 31  2024 .bashrc
-rw-r-----  1 bandit12 bandit11   49 Sep 19 07:08 data.txt
-rw-r--r--  1 root     root      807 Mar 31  2024 .profile
```

I’ve already explained the `ll -a` command in the previous level but I forgot what level that was. This command it’s just the same as `ls -al` command and it will return the same result. Nothing different only the more efficient ways of using. 

```bash
bandit11@bandit:~$ file ./{*,.*}
./data.txt:     ASCII text
./.bash_logout: ASCII text
./.bashrc:      ASCII text
./.profile:     ASCII text
```

Let’s break down this command character by character for better understanding:

**Command:**

```bash
$ file ./{*,.*}
```

- `file`: 
this is the command that determines and displays the type of a file.
- `./` : 
`.` refers to the current directory (In this case is **Home Directory**).
The `/` is a separator, meaning we’re looking for files within the **current directory**.
- `{*,.*}`:
`{}` is **brace expansion**, which generates multiple patterns.
`*` matches **all non-hidden files** in the current directory.
`.*` matches **all hidden files (**files that start with a dot `.`, like `.bashrc` and `.profile`).
- Together, `{*,.*}` expands into a list of all **visible and hidden files** (excluding `.` and `..`).
- So why do we need a comma here? 
Comma`,` **separates different patterns** within `{}`.
`{*,.*}` expands to **both visible and hidden files**.
It prevents unwanted matches like`.` and `..`.

Let’s take a look inside the file using `cat`:

```bash
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

The text doesn’t make sense, which suggests it’s encoded.

One common encoding method in CTF challenges is **ROT13**. ROT13 is a simple cipher that shifts each letter by 13 places. Since I suspect ROT13, I use the `tr` command to **decoce** it.

```bash
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is [REDACTED]
```

- `A-Za-z` → This represents **uppercase (A-Z) and lowercase (a-z)** letters in the alphabet.
- `N-ZA-Mn-za-m` → This shifts each letter forward by 13 places in the alphabet.
- `tr` **(translate)** swaps each letter from **set 1** to its corresponding letter in **set 2**.

Voila! A beautiful password has appeared!

**Below is an example of ROT13:**

```bash
A ↔ N, B ↔ O, C ↔ P, ..., M ↔ Z
N ↔ A, O ↔ B, P ↔ C, ..., Z ↔ M
```

Just in addition:

### **Understanding `tr 'A-Za-z' 'N-ZA-Mn-za-m'`**

This command **performs ROT13 encoding/decoding** by shifting each letter **13 places** forward in the alphabet.

### **1. `tr` - Translate Characters**

`tr` (translate) is a command that replaces characters from **one set** with characters from **another set**.

### **2. `'A-Za-z'` - The First Set (Input)**

- **`A-Z`** → This matches **all uppercase letters** (`A, B, C, ..., Z`).
- **`a-z`** → This matches **all lowercase letters** (`a, b, c, ..., z`).
- Together, `A-Za-z` matches **every letter** in the alphabet.

### **3. `'N-ZA-Mn-za-m'` - The Second Set (Output)**

This defines what each letter will be **translated into**.

- **`N-Z`** → These are **the last 13 uppercase letters** (`N, O, P, ..., Z`).
- **`A-M`** → These are **the first 13 uppercase letters** (`A, B, C, ..., M`).
- **`n-z`** → These are **the last 13 lowercase letters** (`n, o, p, ..., z`).
- **`a-m`** → These are **the first 13 lowercase letters** (`a, b, c, ..., m`).

This means:

- **A becomes N**, **B becomes O**, **C becomes P**, ..., **M becomes Z**.
- **N becomes A**, **O becomes B**, **P becomes C**, ..., **Z becomes M**.
- **Same for lowercase**: **a ↔ n, b ↔ o, ..., m ↔ z**.

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **ROT13 Encoding:** A simpel substitution cipher that shifts letters by 13 places.
- **Using `tr` for character substitution:** `tr 'A-Za-z' 'N-ZA-Mn-za-m'` translates ROT13.
- **Using `file` to check file types: `file data.txt`** identifies the file content type.
- **Brace Expansion `{*,.*}` :** Helps list both visible and hidden files efficiently.
- **Efficient directory listing: `ll -a`**  is an alternative to `ls -al` for listing files, including hidden ones.

## Helpful Reading Material

- [Tr Command](https://www.baeldung.com/linux/tr-command)
- [ROT13](https://simple.wikipedia.org/wiki/ROT13)
- [Rot13 Cipher](https://www.geeksforgeeks.org/rot13-cipher/)
- [Another Tr Command](https://linuxhandbook.com/tr-command/)
