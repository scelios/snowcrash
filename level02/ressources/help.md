-> ls -la

a file that seams interesting is in here :

"level02.pcap"

-> tcpdump -r level02.pcap

tcpdump displays the packages one by one so ...

a lot of lines appear

so from the host i'll copy the file to be able to use Wireshark

-> scp -P 22222 level02@localhost:level02.pcap level02

Now that I can check the file locally

-> strings level02.pcap | grep -i "login"

wwwbugs login: @f&NV.
Login incorrect
wwwbugs login: df&N

-> strings level02.pcap | grep -i "pass"

Password: Nf&Nat

-> su flag02

Password: 
su: Authentication failure

Oupsy..

-> Lets go to wireshark

Once wireshark is installed go to 

file -> open -> level02.pcap

-> Analyze -> Follow -> TCP Stream

Password: ft_wandr...NDRel.LOL

I try the password but failure again :/

in wireshark : 

By checking the hexdump we can see other things:
000000B9  66                                                 f
000000BA  74                                                 t
000000BB  5f                                                 _
000000BC  77                                                 w
000000BD  61                                                 a
000000BE  6e                                                 n
000000BF  64                                                 d
000000C0  72                                                 r
000000C1  7f                                                 .
000000C2  7f                                                 .
000000C3  7f                                                 .
000000C4  4e                                                 N
000000C5  44                                                 D
000000C6  52                                                 R
000000C7  65                                                 e
000000C8  6c                                                 l
000000C9  7f                                                 .
000000CA  4c                                                 L
000000CB  30                                                 0
000000CC  4c                                                 L
000000CD  0d                                                 .

7f = DEL in ASCII
so we obtain:
ft_waNDReL0L

Password:
Don't forget to launch getflag !

-> getflag

Check flag.Here is your token : kooda2puivaav1idi4f57q8iq

-> level03 ..