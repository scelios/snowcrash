
-> ls -la

Again two interesting lines :

-rwsr-s---+ flag08 level08 ... level08

-rw-------  flag08 flag08  ... token

-> ./level09 token

tpmhr

-> cat token

f4kmm6p|=?p?n??DB?Du{??

let's try this as the password :

-> su flag09

Password:
su: Authentication failure

-> su level10

Password:
su: Authentication failure

Let's try something else :

-> ./level09 'f4kmm6p|=pnDBDu{'

f5op:u?DxwNMP??

Ok let's try with this again. Even if it seems to easy :

-> su flag09

Password:
su: Authentication failure

-> su level10

Password:
su: Authentication failure

It doesn't work neither

When observing the following result :
f5op:u?DxwNMP??

It seemps that the return of the program is the argument given
crypted with a sort of ROT+index

Then maybe finding a way to reverse this will give us the password :

level09@SnowCrash:~$ ./level09 abcdef
acegik
level09@SnowCrash:~$ ./level09 abcdefghi
acegikmoq
level09@SnowCrash:~$ ./level09 ihgfedcba
iiiiiiiii
level09@SnowCrash:~$ ./level09 ihgfedcbaz
iiiiiiiii�
level09@SnowCrash:~$ ./level09 ihgfedcba
iiiiiiiii
level09@SnowCrash:~$ nano token
level09@SnowCrash:~$ ./level09 ZYXWVUTSRQPONMLKJIHGFEDCBA
ZZZZZZZZZZZZZZZZZZZZZZZZZZ

it seems that the algo was only:
while string[i]
    strings[i] += i
    i++

by writting a script that will decrypt token, for example

(The script file is in this same directory named as 'level09Script.py')

-> python level09Script.py token

f3iji1ju5yuevaus41q1afiuq

-> su flag09

Password:
Don't forget to launch getflag !

-> getflag

Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z

-> su level10 ..