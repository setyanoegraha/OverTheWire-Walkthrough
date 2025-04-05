Welcome back, brother. Today is Saturday, 05 April 2025.
94 days have passed, and 271 days remain until next year.
Stop making stupid people famous.

┌──(oubaitori㉿DESKTOP-GBJEDR2)-[~]
└─$ cd OverTheWire-Walkthrough/bandit

┌──(oubaitori㉿DESKTOP-GBJEDR2)-[~/OverTheWire-Walkthrough/bandit]
└─$ vim bandit29(raw).md

┌──(oubaitori㉿DESKTOP-GBJEDR2)-[~/OverTheWire-Walkthrough/bandit]
└─$ git log
commit 49e3e6e211e38c6821771303142fb87ca12cf85e (HEAD -> main, origin/main, origin/HEAD)
Author: setyanoegraha <pandu4688@gmail.com>
Date:   Sat Apr 5 09:27:04 2025 +0700

    docs: add raw walkthrough for bandit28

commit 511423d39901447c35f6312354d431b9905e0219
Author: setyanoegraha <pandu4688@gmail.com>
Date:   Sat Apr 5 09:14:56 2025 +0700

    docs: add raw walkthrough for bandit27

commit 5fb4f31aa52d53275c9cf01ed86323b6c226c048
Author: setyanoegraha <pandu4688@gmail.com>
Date:   Sat Apr 5 08:09:08 2025 +0700

    docs: add my raw walkthrough before the write-up for bandit25 and bandit26

┌──(oubaitori㉿DESKTOP-GBJEDR2)-[~/OverTheWire-Walkthrough/bandit]
└─$

┌──(oubaitori㉿DESKTOP-GBJEDR2)-[~/OverTheWire-Walkthrough/bandit]
└─$ ssh bandit29@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit29@bandit.labs.overthewire.org's password:

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

bandit29@bandit:~$ ls
bandit29@bandit:~$ mktemp -d
/tmp/tmp.USURD5v8wK
bandit29@bandit:~$ cd /tmp/tmp.USURD5v8wK
bandit29@bandit:/tmp/tmp.USURD5v8wK$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit29/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit29-git@localhost's password:
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
bandit29@bandit:/tmp/tmp.USURD5v8wK$ ls
repo
bandit29@bandit:/tmp/tmp.USURD5v8wK$ cd repo
bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ ls
README.md
bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ cat
.git/      README.md
bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>

bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ git checkout dev
branch 'dev' set up to track 'origin/dev'.
Switched to a new branch 'dev'
bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ git log
commit 081ac380883f49b0d9dc76a82c53211ef7ba74b0 (HEAD -> dev, origin/dev)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 19 07:08:41 2024 +0000

    add data needed for development

commit 03aa12c85ea4c1ea170b8e5fe80e55de7853b4db
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Sep 19 07:08:41 2024 +0000

    add gif2ascii

commit 6ac7796430c0f39290a0e29a4d32e5126544b022 (origin/master, origin/HEAD, master)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Sep 19 07:08:41 2024 +0000

    fix username

bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

bandit29@bandit:/tmp/tmp.USURD5v8wK/repo$
