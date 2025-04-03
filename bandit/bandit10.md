# Bandit09 → 10: Extracting Readable Strings

[Challenge](https://overthewire.org/wargames/bandit/bandit10.html)

## Level Description

The goal for this level was to find a password stored in a file named `data.txt` in one of the few human-readable strings, preceded by several ‘=’ characters.

## The Process

Upon logging in as `bandit9`, as usual I check what’s the contents of the home directory with the:

```bash
$ ll -a
total 40
drwxr-xr-x  2 root     root     4096 Sep 19 07:08 ./
drwxr-xr-x 70 root     root     4096 Sep 19 07:09 ../
-rw-r--r--  1 root     root      220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root     root     3771 Mar 31  2024 .bashrc
-rw-r-----  1 bandit10 bandit9 19379 Sep 19 07:08 data.txt
-rw-r--r--  1 root     root      807 Mar 31  2024 .profile
```

It looks like there are files named `data.txt` here, So I want to know what kind of file type this is:

```bash
$ file data.txt
data.txt: data
```

which returns a data type file. I Just want to make sure how many lines are there on the file:

```bash
$ wc -l data.txt
77 data.txt
```

Not so much only 77 lines, let’s go see the contents of the file:

```bash
$ cat data.txt
```

and well! it turns out not a text that can be read by humans. There are many characters I can’t recognize. Below are just the snippets from the contents of `data.txt`:

```bash
���jLZ�Wdg�O�o�h�`��&
ZL�     �w�����s �%Z�[␦t��n�oL���:s��X�M��`"�w��������O?�^�oxGz$W~"�O@��gD����6B���t�!6��Un�)
                                                                                             ������ ��m~k��C6O~O�dq��sޑ�E�d*
                     �$��l���mp�;26_C�u%��
                                          x���W�<B(���3O�O����4�0i�B��TI=��q����_S�O��_ �H)m��c��&f�Z�>Pd­';.�u�:y�z؊�QK���K>��lJJ������]1��P>�␦�<�p*���
!0F���
      ��l{�~�_�I�غB��
                     C;��?�2��q�I��%��1�,wD��9+`1�n����␦����]�.m�F0�_$>S6Pp����P���)�J]epm��DфɁg����K��f��G]u��ާ/K�`�WNق���pfT8��Z���v�.}ӱ�?�qe�����8�Mi       9z�@�C'6��Q܃��8/S��̴��|T[���#R�`V�T��$(�6�Z�?�2q?N�gF��"HW�Ϫ+���%�( ��ǳi_�@2�{k�GN�a�i�7��h'�!�5����ȿY@�f�\�5�n���,�Q��r�r�;�*
��@���������H8�fL'~_�9�v��RƯV(�a        Z       �W���2�߭�A����.y<
                                                                 ��)�x��@��A�G��Gz���$�+����m��
                                                                                               :`*�M�o��~o�������!J��'w��ޚӗ��'E[��j�IC�G��;@U�Y0��3Of����ٜj,�Nu�DT[N��,����ʊz9wo�!��p�C�Y
!F_$�+�W�2���j�X�����M��M�8���~ �U�=�FG�Qч���H�L�r�\a�����5Gfy�(�洺
                     ��9��}UF���(>�X�J63�)n
␦�5#��gB�"}@>bn�O(k$N��zw[@���9��(�L�jVoȩ_S����W��"�y�/eJ�È!a5^�ʲb�%�cɕ�9Z8]˙xM�]��=K�5�w�␦7�ތ!)dv�ҧaE�#l�Q^��eq
         �Cx�~���!���L��
                        ٹؤPo�'�P�osqEAj?f��i����_�����s_�;�D)A'r��x.�T������xE�������1��Նf�Ba��I����W3�>D��卲�7w�Ϲan   g���rTϪ'3pp(␦%�z_�a��Zw��1_�
�s(-Qv��!�Sc��XI*O^��'�E3z�t�������4�/:�BӉX���ZE�OFx��9���R%�X��wjǎ�83k=fQ�.�.�B�J�`c���M!��e��b���0�z�X�x�BՂ^R��N�usTR�*��iF�_;~�K␦�����p"|�T{(�B}��;0��yS,�9��Nyz3wI�]G[M[��(%�,�)({� /
```

I need to find some way to get the human-readable strings since there are some characters I can read. Maybe I’ll learn a new command here. Ultimately, after reading some material, I’m going to use the `strings` command, If you wonder what is it, just type `whatis strings` are on a terminal and you’ll get a brief description of that command and type the `strings --help` if you need the help of how to use this command.

Since I can combine one command with another I’ll use the `grep` command to get the specific `=`character, cause in the early, the description tells us the password is preceded by several `=` characters. I type this command to retrieve the password:

```bash
$ strings data.txt | grep ==
}========== the
3JprD========== passwordi
~fDV3========== is
D9========== [REDACTED]
```

Voila! Beautiful password to the next level!

## Password for the Next Level

`[REDACTED]`

## What I Learned

- **Listing Files with `ll -a`:** Using `ll -a` (or `ls -la`) helps reveal all files, including hidden ones, along with their permissions and ownership.
- **Extracting Readable Strings with `strings` :** The `strings` command filters out human-readable text from binary or data files.
- **Finding Specific Patterns with `grep`** – Using `grep` allows searching for patterns, in this case, filtering lines that contain `=` to locate the password.
- **Combining Commands with Pipes (`|`)** – Piping `strings` into `grep` is an efficient way to refine output and extract relevant information.
- **Using `whatis` and `--help` for Learning Commands** – The `whatis` and `--help` options provide brief descriptions and usage information for unfamiliar commands.

## Helpful Reading Material

- [Linux Commands](https://www.geeksforgeeks.org/linux-commands/)
- [Basic Linux Commands](https://www.geeksforgeeks.org/basic-linux-commands/)
- [Strings Command in Linux](https://linuxopsys.com/strings-command-in-linux)
- `man strings`
- `man grep`
- [Grep Command in Linux](https://staging2.linuxopsys.com/grep-command-in-linux)
