

www. `---` ver '---' he '---" ire.org

  
  

Welcome to OverTheWire!

  

If you find any problems, please report them to Steven or morla on

irc.overthewire.org.

  

--[ Playing the games ]--

  

This machine might hold several wargames.

If you are playing "somegame", then:

  

* USERNAMES are somegame0, somegame1, ...

* Most LEVELS are stored in /somegame/.

* PASSWORDS for each level are stored in /etc/somegame_pass/.

  

Write-access to homedirectories is disabled. It is advised to create a

working directory with a hard-to-guess name in /tmp/. You can use the

command "mktemp -d" in order to generate a random and hard to guess

directory in /tmp/. Read-access to both /tmp/ and /proc/ is disabled

so that users can not snoop on eachother. Files and directories with

easily guessable or short names will be periodically deleted!

  

Please play nice:

  

* don't leave orphan processes running

* don't leave exploit-files laying around

* don't annoy other players

* don't post passwords or spoilers

* again, DONT POST SPOILERS!

This includes writeups of your solution on your blog or website!

  

--[ Tips ]--

  

This machine has a 64bit processor and many security-features enabled

by default, although ASLR has been switched off. The following

compiler flags might be interesting:

  

-m32 compile for 32bit

-fno-stack-protector  disable ProPolice

-Wl,-z,norelro disable relro

  

In addition, the execstack tool can be used to flag the stack as

executable on ELF binaries.

  

Finally, network-access is limited for most levels by a local

firewall.

  

--[ Tools ]--

  

For your convenience we have installed a few usefull tools which you can find

in the following locations:

  

* gef (https://github.com/hugsy/gef) in /usr/local/gef/

* pwndbg (https://github.com/pwndbg/pwndbg) in /usr/local/pwndbg/

* peda (https://github.com/longld/peda.git) in /usr/local/peda/

* gdbinit (https://github.com/gdbinit/Gdbinit) in /usr/local/gdbinit/

* pwntools (https://github.com/Gallopsled/pwntools)

* radare2 (http://www.radare.org/)

* checksec.sh (http://www.trapkit.de/tools/checksec.html) in /usr/local/bin/checksec.sh

  

--[ More information ]--

  

For more information regarding individual wargames, visit

http://www.overthewire.org/wargames/

  

For support, questions or comments, contact us through IRC on

irc.overthewire.org #wargames.

  

Enjoy your stay!

  

## Command

ssh bandit@bandit.labs.overthewire.org -p 2220

  

## Bandit 1

pw in readme

boJ9jbbUNNfktd78OOpsqOltutMc3MY1

  

## Bandit 2

readme called -, can’t be called with cat directly because - is a special character that diverts input to echo, can be opened with full direction $ ~/-

  

## Bandit 3

readme with spaces, can easily be called with tab completion (though I guess the trick is to escape the whitespace with \)

  

## Bandit 4

hidden file, found with ls -a

  

## Bandit 5

same as 2, pw in -file07

  

## Bandit 6

use find with proper flags: find -readable -size 1033c

PW: DXjZPULLxYr17uwoI01bNLQbtFemEgo7

  

## Bandit 7

find -size 33c -group bandit6 -user bandit7

PW: HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

## Bandit 8

cat data.txt | grep millionth

pw: cvX2JJa4CFALtqS87jk27qwqGhBM9plV

  

## Bandit 9

Uniq only filters adjacent lines so the file has to be sorted first

cat data.txt | sort | uniq -u

Pw: UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

  

## Bandit 10

strings data.txt | grep ==

Pw: truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

  

## Bandit 11

base64 -d

Pw: IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

  

## Bandit 12

cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

Pw: 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu

  

## Bandit 13

$ xxd -r data.txt > datareverse - write results from reversing the hex dump into datareverse

from there on its always checking the file type $ file /filename/ and unzipping with

$ gunzip /filename/

$ tar xvf /filename/

$ bzip2 -r /filename/

gzip is sensitive to the filetype so if it hasnt’t been named correctly, rename with:

mv wrongfile rightfile.gz

Pw: 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

[https://overthewire.org/wargames/bandit/bandit14.html](https://overthewire.org/wargames/bandit/bandit14.html)

[https://help.ubuntu.com/community/SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)

[https://medium.com/@Kan1shka9/overthewire-wargames-bandit-walkthrough-df2b86826c67](https://medium.com/@Kan1shka9/overthewire-wargames-bandit-walkthrough-df2b86826c67)

  

## Bandit 14

from Bandit 13: $ssh -i ./sshkey.private bandit14@localhost

pw: 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEwMTk2NzgyMV19
-->