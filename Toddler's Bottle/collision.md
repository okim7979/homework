# [Toddler's Bottle] collision
collision에서 제공하는 글은 다음과 같다
```
Daddy told me about cool MD5 hash collision today.
I wanna do something like that too!

ssh col@pwnable.kr -p2222 (pw:guest)
```
putty를 이용해서 접속해주자.

```
col@ubuntu:~$ ls
col  col.c  flag
```
이번에는 `col` `col.c` `flag`가 있음을 확인 할 수 있다.

`col.c` 내용을 확인해보자.
```
#include <stdio.h>
#include <string.h>
unsigned long hashcode = 0x21DD09EC;
unsigned long check_password(const char* p){
        int* ip = (int*)p;
        int i;
        int res=0;
        for(i=0; i<5; i++){
                res += ip[i];
        }
        return res;
}

int main(int argc, char* argv[]){
        if(argc<2){
                printf("usage : %s [passcode]\n", argv[0]);
                return 0;
        }
        if(strlen(argv[1]) != 20){
                printf("passcode length should be 20 bytes\n");
                return 0;
        }

        if(hashcode == check_password( argv[1] )){
                system("/bin/cat flag");
                return 0;
        }
        else
                printf("wrong passcode.\n");
        return 0;
}
```
argc 개수는 2개이하로 제한되어 있으며 argv[1]은 20바이트로 제한되어있다.  
그리고 `hashcode == check_password( argv[1])`로 인해 성립하면 flag가 아니면 패스워드가 틀렸음을 알려준다.

`check_password`함수를 살펴보자.
```
int* ip = (int*)p;
```
위 코드가 의미하는 바를 알아보면 `int`는 4바이트이고 `char`은 1바이트 이므로  
넘겨온 argv[1]값을 4바이트씩 나누어서 ip에 저장한다는것을 알 수 있다.  
그리고 `for` 반복문에서 4바이트씩 나눈 값을 모두 더해주고 더한값을 리턴해준다.

즉, argv[1]값을 4바이트씩 나눠줘서 더해준 값이 `unsigned long hashcode = 0x21DD09EC;`과 동일하면 된다.   
`6C5CEC8x5==21DD09E8` 이므로  `6C5CEC8x4 + 6C5CECC == 21DD09EC`가 성립힌다.

```
col@ubuntu:~$ ./col `perl -e 'print "\xc8\xce\xc5\x06"x4,"\xcc\xce\xc5\x06"'`
daddy! I just managed to create a hash collision :)
```
