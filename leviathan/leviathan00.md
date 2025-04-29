# Leviathan0 -> 1: The Hidden Bookmark

Challenge: `leviathan0@leviathan.labs.overthewire.org -p 2223`

---

## Level Description

This is the starting point of the leviathan wargame. The task is to find the password for the level, `leviathan2`, by exploring the environment on this level.

---

## The Process

After logging into the server:
```bash
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
```

I ran `ll -a` to look for hidden files or directories in the home directory. One directory stood out:
```bash
.backup/
```

I entered the directory:
```bash
cd backup
ll -a
```

Inside, there was one file: `bookmarks.html`. I checked the file type:
```bash
file bookmarks.html
```

It was a regular HTML file. Since it was readable and it said `with very long lines`, I tried to read it using `less`:
```bash
less bookmarks.html
```

Well, indeed the lines were long and cluttered, so I decided to used `cat` and `grep` to search for anything that mentioned the next level:
```bash
cat bookmarks.html | grep leviathan
```

Voila!, that returned the golden ticket!:
```bash
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is [REDACTED]" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

Now I could log in as `leviathan1` using this password.

---

## What I Learned
- Always check for hidden files and directories (`ll -a`).
- Use `grep` to filter large files for spesific keywords.
- Passwords can be hidden in plain text, even in seemingly harmless files like `.html`.


## Helpful Reading Material
- [Hidden Files in Linux](https://www.geeksforgeeks.org/how-to-view-and-create-hidden-files-in-linux/)
- [Using Grep](https://www.geeksforgeeks.org/grep-command-in-unixlinux/)
- [Pipelines](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)

