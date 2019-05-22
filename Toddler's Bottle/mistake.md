# [Toddler's Bottle] mistake
mistake에서 제공하는 글을 확인해보자.

```
We all make mistakes, let's move on.
(don't take this too seriously, no fancy hacking skill is required at all)

This task is based on real event
Thanks to dhmonkey

hint : operator priority

ssh mistake@pwnable.kr -p2222 (pw:guest)
```
putty로 접속해주자.

```
mistake@ubuntu:~$ ls
flag  mistake  mistake.c  password
```
이번에는 password라는 파일이 하나 추가되어있음을 알 수 있다.
```
-r--------  1 mistake_pwn root      10 Jul 29  2014 password
```
그러나 지금 상황에서는 password로는 아무것도 할 수 없는것을 확인했다.

평소대로 `mistake.c` 파일을 확인해보자
```
mistake@ubuntu:~$ cat mistake.c
#include <stdio.h>
#include <fcntl.h>

#define PW_LEN 10
#define XORKEY 1

void xor(char* s, int len){
        int i;
        for(i=0; i<len; i++){
                s[i] ^= XORKEY;
        }
}

int main(int argc, char* argv[]){

        int fd;
        if(fd=open("/home/mistake/password",O_RDONLY,0400) < 0){
                printf("can't open password %d\n", fd);
                return 0;
        }

        printf("do not bruteforce...\n");
        sleep(time(0)%20);

        char pw_buf[PW_LEN+1];
        int len;
        if(!(len=read(fd,pw_buf,PW_LEN) > 0)){
                printf("read error\n");
                close(fd);
                return 0;
        }

        char pw_buf2[PW_LEN+1];
        printf("input password : ");
        scanf("%10s", pw_buf2);

        // xor your input
        xor(pw_buf2, 10);

        if(!strncmp(pw_buf, pw_buf2, PW_LEN)){
                printf("Password OK\n");
                system("/bin/cat flag\n");
        }
        else{
                printf("Wrong Password\n");
        }

        close(fd);
        return 0;
}
```
힌트가 operator priority라고 하였으므로 연산자 우선순위를 생각하며 코드를 살펴보았다.

> 연산자 우선순위 : https://ko.cppreference.com/w/cpp/language/operator_precedence

그 결과
```
if(fd=open("/home/mistake/password",O_RDONLY,0400) < 0){
                printf("can't open password %d\n", fd);
                return 0;
        }
```
과
```
if(!(len=read(fd,pw_buf,PW_LEN) > 0)){
                printf("read error\n");
                close(fd);
                return 0;
        }
```
에서 오류가 있음을 유추할 수 있었다.

아마 개발의도는 open(), read()의 반환 값을 fd, len에 넣고, 그 값을 0보다 작은 지, 큰 지 비교하려는 의도였을 것이다.  
그러나 우선순위에 의해  
open < 0 값이 fd로 들어가게 되었고  
read > 0 값이 len으로 들어가게 되었다.  
따라서 fd가 0이 됨으로써 입력을 두번 받게 되었다.

즉, pw1 == pw2입력값^1 이 되게 입력해주면 된다.
```
>>> ord('b')
98
>>> ord('b')^1
99
>>> ord('b')^ord('c')
1
.
.
mistake@ubuntu:~$ ./mistake
do not bruteforce...
bbbbbbbbbb
cccccccccc
input password : Password OK
Mommy, the operator priority always confuses me :(
```
