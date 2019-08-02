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
또한 NX가 켜져있는 것을 보아 
