
-> ls -la

the next two lines seems exploitable :

-rwsr-sr-x+ flag10 level10 ... level10

-rw-------  flag10 flag10  ... token

-> ./level10 token

./level10 file host
    sends file to host if you have access to it

-> strings level10 > output.txt

-> scp -P XXXX level10@localhost:/home/user/level10/output.txt /level10/resources/

when checking the readable lines in output.txt (file available in this directory),
we can see that the executable verifies whether we have permission to read the file
using access, then attempts to send it to a server and display its contents.

Taking in account that we lack the required rights for the token file, we need to
find a way to exploit the executable in our favor. 

So, as the executable does something like :
access(path, R_OK)
open(path, O_RDONLY)

-> man access

NOTES
       Warning:  Using access() to check if a user is authorized to, for example, open a
       file before actually doing so using open(2) creates a security hole, because  the
       user  might exploit the short time interval between checking and opening the file
       to manipulate it.  For this reason,  the  use  of  this  system  call  should  be
       avoided.   (In the example just described, a safer alternative would be to tempo‐
       rarily switch the process's effective user ID  to  the  real  ID  and  then  call
       open(2).)

Let's used this loophole to get access to the token.

For that we will constantly create a file, delete it and create a symlink with the same name of the file between the file and the token

There is a known TOCTOU (Time Of Check / Time Of Use) vulnerability.

with a symbolic link: between the check and the open, let's replace the target with a
sensitive file. Result: access() passes (on a “harmless” file), but open() reads the
real sensitive file, and the binary sends it to us over the network (port 6969).

To do so, we'll need 3 different terminal connected to the SnowCrash VM :

Terminal 1 : nc -lk 6969

Terminal 2 : run linkScript.sh

Terminal 3 : run funScript.sh

CTRL+C on each terminal

On the first terminal we can see this output :

.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
woupa2yuojeeaaed06riuj63c
.*( )*.
.*( )*.
.*( )*.
.*( )*.
.*( )*.
woupa2yuojeeaaed06riuj63c

-> su flag10

Password: 
Don't forget to launch getflag !

-> getflag

Check flag.Here is your token : feulo4b72j7edeahuete3no7c

-> su level11 ..