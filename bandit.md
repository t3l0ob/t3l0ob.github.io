# my writeup for bandit CTF have fun

- took a lot of influence from here
https://home.adelphi.edu/~ni21347/cybersecgames/OverTheWire/Bandit/index.html
https://mayadevbe.me/

---
Host: bandit.labs.overthewire.org

Port: 2220

## bandit level 0
http://overthewire.org/wargames/bandit/bandit0.html

In their website they give us the username and password for bandit0 and we have to find the password for bandit1

**username:** bandit0
**password:** bandit0

---

## bandit 0 → 1
http://overthewire.org/wargames/bandit/bandit1.html

```sh
ls
cat readme 
```

**username:** bandit1
**password:** NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

---

## bandit 1 → 2
http://overthewire.org/wargames/bandit/bandit2.html

The password for the next level is stored in a file called readme located in the home directory

```sh
ls
cat ./-
cat <-
```

**username:** bandit2
**password:** rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

**Reference:** https://unix.stackexchange.com/questions/16357/usage-of-dash-in-place-of-a-filename
**Reference:** [Advanced Bash-scripting Guide - Chapter 3 - Special Characters](https://tldp.org/LDP/abs/html/special-chars.html)

---

## bandit 2 → 3
http://overthewire.org/wargames/bandit/bandit3.html

The password for the next level is stored in a file called spaces in this filename located in the home directory

```sh
cat "spaces in this filename"
cat space\ in\ this\ filename
```
**username:** bandit3
**password:** aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

---

## bandit 3 → 4
http://overthewire.org/wargames/bandit/bandit4.html

The password for the next level is stored in a hidden file in the inhere directory.

```sh
ls -a hidden
cat inhere/.hidden # 
```
**username:** bandit4
**password:** 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe 

---

## bandit 4 → 5
http://overthewire.org/wargames/bandit/bandit5.html

The password for the next level is stored in the only human-readable file in the inhere directory

```sh
cd inhere
for x in {0..9}; do file ./-file0$x; done
cat inhere/-file07 
```
**username:** bandit5
**password:** lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

---

## bandit 5 → 6
http://overthewire.org/wargames/bandit/bandit6.html

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
  - human-readable
  - 1033 bytes in size
  - not executable

```sh
cd inhere
find -size 1033c -type f ! -executable
cat maybehere07/.file2
# another way to show the file without the path
ls -laR | grep rw-r | grep 1033
```

**username:** bandit6
**password:** P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

---

## bandit 6 → 7
http://overthewire.org/wargames/bandit/bandit7.html

The password for the next level is stored somewhere on the server and has all of the following properties:
  - owned by user bandit7
  - owned by group bandit6
  - 33 bytes in size

```sh
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

**username:** bandit7
**password:** z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

---

## bandit 7 → 8
http://overthewire.org/wargames/bandit/bandit8.html

The password for the next level is stored in the file data.txt next to the word millionth

```sh
cat data.txt | grep millionth
```
**username:** bandit8
**password:** TESKZC0XvTetK0S9xNwm25STk5iWrBvP

---

## bandit 8 → 9
http://overthewire.org/wargames/bandit/bandit9.html

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

```sh
sort data.txt | uniq -u
```
**username:** bandit9
**password:** EN632PlfYiZbn3PhVK3XOGSlNInNE00t

**Reference:** [Remove duplicate lines with uniq](http://www.liamdelahunty.com/tips/linux_remove_duplicate_lines_with_uniq.php)
**Reference:** [piping and redirection](https://ryanstutorials.net/linuxtutorial/piping.php)

---

## bandit 9 → 10
http://overthewire.org/wargames/bandit/bandit10.html

The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

```sh
cat data.txt | strings | grep ^=
```
**username:** bandit10
**password:** G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

---

## bandit 10 → 11
http://overthewire.org/wargames/bandit/bandit11.html

The password for the next level is stored in the file data.txt, which contains base64 encoded data

```sh
cat data.txt | base64 --decode
```
**username:** bandit11
**password:** 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

---

## bandit 11 → 12
http://overthewire.org/wargames/bandit/bandit12.html

The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

The data.txt file contains 1 line that was encrypted with the ROT13 algorithm. In order to decrypt it, I have to replace every letter by the letter 13 positions ahead. For example, with this encryption the letter a would be replaced with n. The word banana would encrypt to onanan. To decrypt the line I ran the command below:

```sh
tr '[A-Za-z]' '[N-ZA-Mn-za-m]' < file
```
**username:** bandit12
**password:** JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

**Reference:** [ROT13](https://en.wikipedia.org/wiki/ROT13)

---

## bandit 12 → 13
http://overthewire.org/wargames/bandit/bandit13.html

The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed.

```sh
mkdir /tmp/zzz && copy data.txt /tmp/zzz
cd /tmp/zzz
xxd -r file outputFile
# first rename the file so gzip can decopress it
mv outputFile outputFile.gz
gzip -d outputFile.gz
# you can omit this step but the output will be file.out
mv outputFile outputFile.bz2
bzip2 -d outputFile.bz2
mv outputFile outputFile.gz
gzip -d outputFile.gz
tar -xf outputFile
tar -xf outputFile
mv outputFile outputFile.bz2
bzip2 -d outputFile.bz2
tar -xf outputFile
mv outputFile outputFile.gz
gzip -d outputFile.gz
cat data.txt
```
**username:** bandit13
**password:** wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw

---

## bandit 13 → 14
http://overthewire.org/wargames/bandit/bandit14.html

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level.

```sh
ssh -i sshkey.private bandit14@localhost -p 2220
cat /etc/bandit_pass/bandit14
```
**username:** bandit14
**password:** fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq

---

## bandit 14 → 15
http://overthewire.org/wargames/bandit/bandit15.html

The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

```sh
echo "fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq" | nc localhost 30000
```
**username:** bandit15
**password:** jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

---

## bandit 15 → 16
http://overthewire.org/wargames/bandit/bandit16.html

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…


```sh
openssl s_client -connect localhost:30001 -ign_eof
```

**username:** bandit16
**password:** JQttfApK4SeyHwDlI9SXGR50qclOAil1

---

## bandit 16 → 17
http://overthewire.org/wargames/bandit/bandit17.html

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

```sh
nmap -v -A -T4 -p 31000-32000 localhost
openssl s_client -connect localhost:31790
```

you get a private RSA key

> -----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

```sh
mkdir /tmp/test && cd /tmp/test
# save the rsa key in a file using vim
vim bandit17.key
# the identity file will get rejected if it has too many permissions
chmod 600 bandit17.key
ssh -i bandit17.key bandit17@localhost -p 2220
```

**username:** bandit17
**password:** VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e

---

## bandit 17 → 18
http://overthewire.org/wargames/bandit/bandit18.html

There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

```sh
diff password.old password.new
```

**username:** bandit18
**password:** hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg

---

## bandit 18 → 19
http://overthewire.org/wargames/bandit/bandit19.html

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

```sh
ssh -t -p 2220 bandit18@localhost "/bin/bash --norc"
```

**username:** bandit19
**password:** awhqfNnAbc1naukrpqDYcF95h7HoMTrC

---

## bandit 19 → 20
http://overthewire.org/wargames/bandit/bandit20.html

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

```sh
./bandit20-do cat /etc/bandit_pass/bandit20
```
**username:** bandit20
**password:** VxCazJaVykI6W36BkBU0mJTCM8rR95XT

---

## bandit 20 → 21
http://overthewire.org/wargames/bandit/bandit21.html

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

```sh
tmux
nc -lvp 1234 < /etc/bandit_pass/bandit20
./suconnect 1234
```
**username:** bandit21
**password:** NvEJF7oVjkddltPSrdKEFOllh9V1IBcq

---

## bandit 21 → 22
http://overthewire.org/wargames/bandit/bandit22.html

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

```sh
cd /etc/cron.d/
cat cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
**username:** bandit22
**password:** WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff

---

## bandit 22 → 23
http://overthewire.org/wargames/bandit/bandit23.html

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

```sh
cd /etc/cron.d/
cat cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```
**username:** bandit23
**password:** QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G

---

## bandit 23 → 24
http://overthewire.org/wargames/bandit/bandit24.html

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

- cron supposed to run jobs of each user after they log in, i take advantage of the fact that a script is going to run

```sh
cd /etc/cron.d/
cat cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh
mkdir -p /tmp/test && cd /tmp/test
# create a shell script and make it so anyone can run it
vim script.sh
chmod 777 script.sh
cp script.sh /var/spool/bandit24/foo/
cat /tmp/result
```
**username:** bandit24
**password:** VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar

---

## bandit 24 → 25
http://overthewire.org/wargames/bandit/bandit25.html

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

```sh
for i in {0000..9999}
do
    echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i"
done | netcat localhost 30002
```
**username:** bandit25
**password:** p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

---

## bandit 25 → 26
http://overthewire.org/wargames/bandit/bandit26.html

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

`more` is a shell command that allows the display of files in an interactive mode. Specifically, this interactive mode only works when the content of the file is too large to fully be displayed in the terminal window. One command that is allowed in the interactive mode is `v`. This command will open the file in the editor `vim`.

Vim is a text editor. It enables you to run shell commands as well. It is possible to use vim to break out of a restricted environment and spawn a shell. To spawn the user’s default shell, the command `:shell` is used. To change the shell to ‘/bin/bash’ the command is `:set shell=/bin/sh`

```sh
cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
ls -la /usr/bin/showtext
-rwxr-xr-x 1 root root 53 May  7  2020 /usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```
When trying to log in, we see that the connection is closed because ‘/usr/bin/showtext’ is executed.

```sh
$ ssh -i bandit26.sshkey bandit26@localhost
...
  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to bandit.labs.overthewire.org closed.
```
What exactly has happened? The text in ’text.txt’ is very short, meaning the whole text can immediately be displayed. more does not need to go into command/interactive mode. If we make the terminal window smaller, more will go into command mode. We can then use v to go into vim. Now we can rescale the window.

Vim is now opened as bandit26 and we can do different things to retrieve the password. With `:e /etc/bandit_pass/bandit26` we can open the password file and read the password. If we want a shell, we could try the :shell command that vim offers. This command, however, uses the user’s default shell. What we need to do instead is to set the default shell of the user in vim to a useful shell, like `\bin\bash`. The commands look like the following: `:set shell=/bin/bash` and then use `:shell`. Finally, we have a shell and can get the password for the user.

```sh
v
:e /etc/bandit\_pass/bandit26
# OR even better a shell
# resize
:set shell=/usr/bin/bash
:shell
```


**username:** bandit26
**password:** c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1

**Reference:** https://mayadevbe.me/posts/overthewire/bandit/level26/

---

## bandit 26 → 27
http://overthewire.org/wargames/bandit/bandit27.html

Good job getting a shell! Now hurry and grab the password for bandit27!

```sh
ls
./bandit27-do cat /etc/bandit_pass/bandit27
```
**username:** bandit27
**password:** YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS

---

## bandit 27 → 28
http://overthewire.org/wargames/bandit/bandit28.html

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.

Clone the repository and find the password for the next level.

```sh
mktemp -d
/tmp/tmp.G0PJwS3hFT
cd /tmp/tmp.G0PJwS3hFT
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
cat repo/README
```
**username:** bandit28
**password:** AVanL161y9rsbcJIsFHuw35rjaOM19nR

---

## bandit 28 → 29
http://overthewire.org/wargames/bandit/bandit29.html

There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

```sh
mktemp -d
/tmp/tmp.G0PJwS3hFT
cd /tmp/tmp.G0PJwS3hFT
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
cat repo/README
git log
git show 14f754b3ba6531a2b89df6ccae6446e8969a41f3
# next step is not neaded
git revert HEAD
cat README.md
```
**username:** bandit29
**password:** tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S

---

## bandit 29 → 30
http://overthewire.org/wargames/bandit/bandit30.html

There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo` via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.

```sh
mktemp -d
/tmp/tmp.G0PJwS3hFT
cd /tmp/tmp.G0PJwS3hFT
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
cd repo
cat README
git branch -a
git checkout dev
cat README.md
```
**username:** bandit30
**password:** xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS

---

## bandit 30 → 31
http://overthewire.org/wargames/bandit/bandit31.html

There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo` via the port 2220. The password for the user bandit29-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.

```sh
mktemp -d
/tmp/tmp.G0PJwS3hFT
cd /tmp/tmp.G0PJwS3hFT
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
cd repo
cat README
git tag
git show secret
cat README.md
```
**username:** bandit31
**password:** OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt

---

## bandit 31 → 32
http://overthewire.org/wargames/bandit/bandit32.html

There is a git repository at `ssh://bandit31-git@localhost/home/bandit31-git/repo` via the port 2220. The password for the user bandit29-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

```sh
mktemp -d
/tmp/tmp.G0PJwS3hFT
cd /tmp/tmp.G0PJwS3hFT
git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
cd repo
cat README
echo 'May I come in?' > key.txt
git add -f .
git commit -m "adding text file in the git ignore"
git push -u origin master
```
**username:** bandit32
**password:** rmCBvG56y58BXzv98yZGdO7ATVL5dW8y

---

## bandit 32 → 33
http://overthewire.org/wargames/bandit/bandit33.html

After all this git stuff its time for another escape. Good luck!

```sh
$0
cat /etc/bandit_pass/bandit33
```
**username:** bandit33
**password:** odHo63fHiFqcWWJG9rLiLDtPm45KzUKy

**Reference:** https://mayadevbe.me/posts/overthewire/bandit/level33/

---

## bandit 33 → 34
http://overthewire.org/wargames/bandit/bandit34.html


At this moment, level 34 does not exist yet.

```sh
cat README.txt
```

Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!

---

- > iam alice said alice the girl named alice said abrubtly
    zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz .......
