-> ls -la
Nothing seems exploitable here

The 3 visible files aren't of interest

So I'll check for files belonging to flag00

-> find / -user flag00

A lot of lines appear. To many to be able to check each one

So, maybe filtering the error messages could help

-> find / -user flag00 2>/dev/null

(also the command "find / -group flag00 2>/dev/null" could work)

1 file appear and it seems to contain the answer

-> cat /usr/sbin/john

The following text appears

cdiiddwpgswtgt

-> su flag00

The password is asked and I put the text found in the file

"su: Authentication failure"

I try with the hexa in the file

-> xxd /usr/sbin/john

This texte appears 

"0000000: 6364 6969 6464 7770 6773 7774 6774 0a    cdiiddwpgswtgt."

-> xxd -p /usr/sbin/john

I try the following combination with su flag00

63646969646477706773777467740a

"su: Authentication failure"

Then I decide to go to dcode.fr and check the different tools there

I try a few things without success until I red the search bar "Tapez par exemple 'cesar'"

Then I take a look to the tool :

The Cesar code is a monoalphabetic substitution cipher, where each letter is replaced by another letter located a little further along in the alphabet (i.e., shifted but always identical for the same message).

I put the encrypted code there and the first output is :

nottoohardhere

-> su flag00

-> nottoohardhere

level00@SnowCrash:~$ su flag00
Password: 
Don't forget to launch getflag !

-> getflag

Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias

-> su level01