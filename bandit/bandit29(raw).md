┌──(oubaitori㉿DESKTOP-GBJEDR2)-[~]
└─$ ssh bandit28@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit28@bandit.labs.overthewire.org's password:

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit28@bandit:~$ ls
bandit28@bandit:~$ mktemp -d
/tmp/tmp.Ap3czh0WXI
bandit28@bandit:~$ cd /tmp/tmp.Ap3czh0WXI
bandit28@bandit:/tmp/tmp.Ap3czh0WXI$ git clone ssh://bandit28-git@localhost:2220/h
ome/bandit28-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit28/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit28/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit28-git@localhost's password:
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
bandit28@bandit:/tmp/tmp.Ap3czh0WXI$ ls
repo
bandit28@bandit:/tmp/tmp.Ap3czh0WXI$ cd repo
bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo$ ll -a
total 16
drwxrwxr-x 3 bandit28 bandit28 4096 Apr  5 09:29 ./
drwx------ 3 bandit28 bandit28 4096 Apr  5 09:28 ../
drwxrwxr-x 8 bandit28 bandit28 4096 Apr  5 09:29 .git/
-rw-rw-r-- 1 bandit28 bandit28  111 Apr  5 09:29 README.md
bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx

bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo$ cd .git/
bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo/.git$ ll -a
total 52
drwxrwxr-x 8 bandit28 bandit28 4096 Apr  5 09:29 ./
drwxrwxr-x 3 bandit28 bandit28 4096 Apr  5 09:29 ../
drwxrwxr-x 2 bandit28 bandit28 4096 Apr  5 09:28 branches/
-rw-rw-r-- 1 bandit28 bandit28  281 Apr  5 09:29 config
-rw-rw-r-- 1 bandit28 bandit28   73 Apr  5 09:28 description
-rw-rw-r-- 1 bandit28 bandit28   23 Apr  5 09:29 HEAD
drwxrwxr-x 2 bandit28 bandit28 4096 Apr  5 09:28 hooks/
-rw-rw-r-- 1 bandit28 bandit28  137 Apr  5 09:29 index
drwxrwxr-x 2 bandit28 bandit28 4096 Apr  5 09:28 info/
drwxrwxr-x 3 bandit28 bandit28 4096 Apr  5 09:29 logs/
drwxrwxr-x 4 bandit28 bandit28 4096 Apr  5 09:28 objects/
-rw-rw-r-- 1 bandit28 bandit28  114 Apr  5 09:29 packed-refs
drwxrwxr-x 5 bandit28 bandit28 4096 Apr  5 09:29 refs/
bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo/.git$ git log
commit 817e303aa6c2b207ea043c7bba1bb7575dc4ea73 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    fix info leak

commit 3621de89d8eac9d3b64302bfb2dc67e9a566decd
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    add missing data

commit 0622b73250502618babac3d174724bb303c32182
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    initial commit of README.md
bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo/.git$ git show 817e303aa6c2b207ea043c7bba1bb7575dc4ea73
commit 817e303aa6c2b207ea043c7bba1bb7575dc4ea73 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
+- password: xxxxxxxxxx

bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo/.git$ git show 0622b73250502618babac3d174724bb303c32182
commit 0622b73250502618babac3d174724bb303c32182
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..7ba2d2f
--- /dev/null
+++ b/README.md
@@ -0,0 +1,8 @@
+# Bandit Notes
+Some notes for level29 of bandit.
+
+## credentials
+
+- username: bandit29
+- password: <TBD>
+
bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo/.git$ git show 3621de89d8eac9d3b64302bfb2dc67e9a566decd
commit 3621de89d8eac9d3b64302bfb2dc67e9a566decd
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..d4e3b74 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: <TBD>
+- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

bandit28@bandit:/tmp/tmp.Ap3czh0WXI/repo/.git$
