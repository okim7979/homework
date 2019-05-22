# [Toddler's Bottle] fd
fd에서 제공하는 글을 확인해보면 다음과 같다.
```
Mommy! what is a file descriptor in Linux?

* try to play the wargame your self but if you are ABSOLUTE beginner, follow this tutorial link:
https://youtu.be/971eZhMHQQw

ssh fd@pwnable.kr -p2222 (pw:guest)
```
putty를 이용하여 접속을 해주자.

그리고 ls명령어를 통해 어떤 것들이 있나 확인을 해보자.
```
fd@ubuntu:~$ ls
fd  fd.c  flag
```

fd.c 안에 있는 내용을 살펴보았더니 다음과 같은 c코드가 적혀있었다.
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
        if(argc<2){
                printf("pass argv[1] a number\n");
                return 0;
        }
        int fd = atoi( argv[1] ) - 0x1234;
        int len = 0;
        len = read(fd, buf, 32);
        if(!strcmp("LETMEWIN\n", buf)){
                printf("good job :)\n");
                system("/bin/cat flag");
                exit(0);
        }
        printf("learn about Linux file IO\n");
        return 0;

}
```
fd라는 프로그램은 인자를 1개까지만 받는다는 것을 알 수 있고   
read 함수를 통해 buf에 있는 내용을 32바이트만큼 받아온다는것을 알 수 있다.
```
#include <unistd.h>
　　size_t read( int fildes, void *buf, size_t nbytes );
첫번째 인자 :: fildes  :: 파일 서술자
두번째 인자 :: *buf    :: 읽어와서 저장할 버퍼.
새번째 인자 :: nbytes :: 읽을 수 있는 최대 byte의 수
```
만약에 첫번째 인자가 파일을 가리키는 서술자였다면, 그 파일에 적혀있는 내용을 buffer에 읽어다.

정수값 | 이름
:------: | :------:
0|표준 입력( stdin ) /dev/stdin
1|표준 출력( stdout ) /dev/stdout
2|표준 오류( stderr )

그러나 우리는 첫번째 인자에 0을 넣어줘서  
표준 입력인 서술자에 대해서 read함수로 읽어오게 만들어 준다.

첫번째 인자인 fd를 0으로 만들어주기 위해서는 `0x1234 == 4660`이므로 argv[1]에 4660을 넣어주면 해결된다.  
그리고 buf가 "LETMEWIN"과 동일한지 비교하는 if문이 있으므로 `LETMEWIN`을 입력해주면 flag가 나오게 된다.
```
fd@ubuntu:~$ ./fd 4660
LETMEWIN
good job :)
mommy! I think I know what a file descriptor is!!
```
