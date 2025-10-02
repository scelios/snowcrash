level14@SnowCrash:~$ ls /var/mail
level05
level14@SnowCrash:~$ ls -l
total 0
level14@SnowCrash:~$ find / -user flag14 2>/dev/null
level14@SnowCrash:~$ cat /etc/passwd
[...]
level13:x:2013:2013::/home/user/level13:/bin/bash
level14:x:2014:2014::/home/user/level14:/bin/bash
flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
flag02:x:3002:3002::/home/flag/flag02:/bin/bash
flag03:x:3003:3003::/home/flag/flag03:/bin/bash
flag04:x:3004:3004::/home/flag/flag04:/bin/bash
flag05:x:3005:3005::/home/flag/flag05:/bin/bash
flag06:x:3006:3006::/home/flag/flag06:/bin/bash
flag07:x:3007:3007::/home/flag/flag07:/bin/bash
flag08:x:3008:3008::/home/flag/flag08:/bin/bash
flag09:x:3009:3009::/home/flag/flag09:/bin/bash
flag10:x:3010:3010::/home/flag/flag10:/bin/bash
flag11:x:3011:3011::/home/flag/flag11:/bin/bash
flag12:x:3012:3012::/home/flag/flag12:/bin/bash
flag13:x:3013:3013::/home/flag/flag13:/bin/bash
flag14:x:3014:3014::/home/flag/flag14:/bin/bash

Since we have nothing let's try to directly change get flag with gdb to get our flag

level14@SnowCrash:~$ gdb getflag
(gdb) run
Starting program: /bin/getflag 
You should not reverse this
[Inferior 1 (process 2562) exited with code 01]
(gdb) disass main
Dump of assembler code for function main:
   0x08048946 <+0>:     push   %ebp
   0x08048947 <+1>:     mov    %esp,%ebp
   0x08048949 <+3>:     push   %ebx
   0x0804894a <+4>:     and    $0xfffffff0,%esp
   0x0804894d <+7>:     sub    $0x120,%esp
   0x08048953 <+13>:    mov    %gs:0x14,%eax
   0x08048959 <+19>:    mov    %eax,0x11c(%esp)
   0x08048960 <+26>:    xor    %eax,%eax
   0x08048962 <+28>:    movl   $0x0,0x10(%esp)
   0x0804896a <+36>:    movl   $0x0,0xc(%esp)
   0x08048972 <+44>:    movl   $0x1,0x8(%esp)
   0x0804897a <+52>:    movl   $0x0,0x4(%esp)
   0x08048982 <+60>:    movl   $0x0,(%esp)
   0x08048989 <+67>:    call   0x8048540 <ptrace@plt> #used of ptrace here
   0x0804898e <+72>:    test   %eax,%eax
   0x08048990 <+74>:    jns    0x80489a8 <main+98>
   0x08048992 <+76>:    movl   $0x8048fa8,(%esp)
   0x08048999 <+83>:    call   0x80484e0 <puts@plt>
   0x0804899e <+88>:    mov    $0x1,%eax
   0x080489a3 <+93>:    jmp    0x8048eb2 <main+1388>
   0x080489a8 <+98>:    movl   $0x8048fc4,(%esp)
   0x080489af <+105>:   call   0x80484d0 <getenv@plt>
   0x080489b4 <+110>:   test   %eax,%eax
   0x080489b6 <+112>:   je     0x80489ea <main+164>
   0x080489b8 <+114>:   mov    0x804b040,%eax
   0x080489bd <+119>:   mov    %eax,%edx
   0x080489bf <+121>:   mov    $0x8048fd0,%eax
   0x080489c4 <+126>:   mov    %edx,0xc(%esp)
   0x080489c8 <+130>:   movl   $0x25,0x8(%esp)
   0x080489d0 <+138>:   movl   $0x1,0x4(%esp)
   0x080489d8 <+146>:   mov    %eax,(%esp)
   0x080489db <+149>:   call   0x80484c0 <fwrite@plt>
   0x080489e0 <+154>:   mov    $0x1,%eax
   0x080489e5 <+159>:   jmp    0x8048eb2 <main+1388>
   0x080489ea <+164>:   movl   $0x0,0x4(%esp)
   0x080489f2 <+172>:   movl   $0x8048ff6,(%esp)
   0x080489f9 <+179>:   call   0x8048500 <open@plt>
   0x080489fe <+184>:   test   %eax,%eax
   0x08048a00 <+186>:   jle    0x8048a34 <main+238>
   0x08048a02 <+188>:   mov    0x804b040,%eax
   0x08048a07 <+193>:   mov    %eax,%edx
   0x08048a09 <+195>:   mov    $0x8048fd0,%eax
   0x08048a0e <+200>:   mov    %edx,0xc(%esp)
   0x08048a12 <+204>:   movl   $0x25,0x8(%esp)
   0x08048a1a <+212>:   movl   $0x1,0x4(%esp)
   0x08048a22 <+220>:   mov    %eax,(%esp)
   0x08048a25 <+223>:   call   0x80484c0 <fwrite@plt>
   0x08048a2a <+228>:   mov    $0x1,%eax
   0x08048a2f <+233>:   jmp    0x8048eb2 <main+1388>
   0x08048a34 <+238>:   movl   $0x0,0x4(%esp)

We can see that the main used ptrace, wich is used to launch another thread, so we can bypass it thanks to this link:
https://gist.github.com/poxyran/71a993d292eee10e95b4ff87066ea8f2

(gdb) catch syscall ptrace
(gdb) commands 1
Type commands for breakpoint(s) 1, one per line.
End with a line saying just "end".
>set ($eax)=0
>continue
>end
(gdb) run
Starting program: /bin/getflag 

Catchpoint 1 (call to syscall ptrace), 0xb7fdd428 in __kernel_vsyscall ()

Catchpoint 1 (returned from syscall ptrace), 0xb7fdd428 in __kernel_vsyscall ()
Check flag.Here is your token : 
Nope there is no token here for you sorry. Try again :)

(gdb) disass main
Dump of assembler code for function main:
   0x08048946 <+0>:     push   %ebp
   [...]
   0x08048989 <+67>:    call   0x8048540 <ptrace@plt>
   [...]
   0x08048a4c <+262>:   cmpl   $0xffffffff,0x14(%esp)
   0x08048a51 <+267>:   jne    0x8048e88 <main+1346>
   0x08048a57 <+273>:   mov    0x804b040,%eax
---Type <return> to continue, or q <return> to quit---
   0x08048a5c <+278>:   mov    %eax,%edx
   [...]
   0x08048afd <+439>:   call   0x80484b0 <getuid@plt>       #getuid used here
   [...]
   0x08048b5d <+535>:   ja     0x8048ca7 <main+865>
   0x08048b63 <+541>:   jmp    0x8048c5f <main+793>
   0x08048b68 <+546>:   cmp    $0xbc2,%eax
---Type <return> to continue, or q <return> to quit---q
Quit

We can see that onve again getuid is used like in level13
So let's try to change the uid with gdb.
But first we have to know the id of flag14

level14@SnowCrash:~$ id flag14
uid=3014(flag14) gid=3014(flag14) groups=3014(flag14),1001(flag)

(gdb) break getuid
Breakpoint 2 at 0xb7ee4cc0
(gdb) run
Starting program: /bin/getflag 

Catchpoint 1 (call to syscall ptrace), 0xb7fdd428 in __kernel_vsyscall ()

Catchpoint 1 (returned from syscall ptrace), 0xb7fdd428 in __kernel_vsyscall ()

Breakpoint 2, 0xb7ee4cc0 in getuid () from /lib/i386-linux-gnu/libc.so.6
(gdb) step
Single stepping until exit from function getuid,
which has no line number information.
0x08048b02 in main ()
(gdb) print $eax
$1 = 2014
(gdb) set $eax=3014
(gdb) step
Single stepping until exit from function main,
which has no line number information.
Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
0xb7e454d3 in __libc_start_main () from /lib/i386-linux-gnu/libc.so.6


level14@SnowCrash:~$ su flag14
Password: 
Congratulation. Type getflag to get the key and send it to me the owner of this livecd :)
flag14@SnowCrash:~$ getflag
Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
