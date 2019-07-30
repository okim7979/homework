lab1
=============

lab1 폴더에 들어가보면   
sysmagic sysmagic.c라는 파일이 있다.
아마 `sysmagic` 실행파일은 `sysmagic.c`를 컴파일한 것으로 보인다.  
`sysmagic.c`의 코드를 살펴보면 다음과 같다.

```
#include <stdio.h>
#include <unistd.h>



void get_flag(){
	int fd ;
	int password;
	int magic ;
	char key[] = "Do_you_know_why_my_teammate_Orange_is_so_angry???";
	char cipher[] = {7, 59, 25, 2, 11, 16, 61, 30, 9, 8, 18, 45, 40, 89, 10, 0, 30, 22, 0, 4, 85, 22, 8, 31, 7, 1, 9, 0, 126, 28, 62, 10, 30, 11, 107, 4, 66, 60, 44, 91, 49, 85, 2, 30, 33, 16, 76, 30, 66};
	fd = open("/dev/urandom",0);
	read(fd,&password,4);
	printf("Give me maigc :");
	scanf("%d",&magic);
	if(password == magic){
		for(int i = 0 ; i < sizeof(cipher) ; i++){
			printf("%c",cipher[i]^key[i]);
		}
	}
}


int main(){
	setvbuf(stdout,0,2,0);
	get_flag();
	return 0 ;
}
```
`get_flag()`를 살펴보면 `password`는 랜덤한 값으로 설정된다는 것을 알 수 있으며 이것이 `magic`값과 같을 때 flag를 보여준다.

gdb로 `get_flag()`의 어셈코드를 확인해 보았으며  
```
   0x08048712 <+375>:	call   0x8048480 <__isoc99_scanf@plt>
   0x08048717 <+380>:	add    esp,0x10
   0x0804871a <+383>:	mov    edx,DWORD PTR [ebp-0x80]
   0x0804871d <+386>:	mov    eax,DWORD PTR [ebp-0x7c]
   0x08048720 <+389>:	cmp    edx,eax
   0x08048722 <+391>:	jne    0x8048760 <get_flag+453>
```
`get_flag()+389` 이 부분이 `if(password == magic)` 이 명령어 부분임을 확인하였다.   
따라서 이 부분에 break를 걸고 실행시켜 본 결과 
![](https://postfiles.pstatic.net/MjAxOTA3MzBfMTY5/MDAxNTY0NDU5MTU2ODI0.Vs9YoJIOrw-mbGODWXfK6IC0lbVxgQWeqUK8z_TuBKog.JnOST_B1mlpM-Dd7M50KUqRahY6-nIB4O870o9tMjyYg.PNG.rlaeoghks823/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,_2019-07-04_17-28-07.png?type=w773)    

`EDX(password)`의 값은 `0x9e8c4441`   
`EAX(magic)`의 값은 `0x1d9d6c`임을 확인하였다.

`set`을 통해 `magic`값을 `password`값과 동일하게 만들어주고 프로그램을 마저 실행시켜주면 다음과같이 플래그를 얻을 수 있다.
```
gdb-peda$ set $eax=0x9e8c4441

gdb-peda$ c
Continuing.
CTF{debugger_1s_so_p0werful_1n_dyn4m1c_4n4lySis!}[Inferior 1 (process 4357) exited normally]
```
