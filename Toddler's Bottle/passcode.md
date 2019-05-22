# [Toddler's Bottle] passcode
passcode에서 제공하는 글을 확인해보자.
```
Mommy told me to make a passcode based login system.
My initial C code was compiled without any error!
Well, there was some compiler warning, but who cares about that?

ssh passcode@pwnable.kr -p2222 (pw:guest)
```
putty를 이용하여 주어진 주소로 접속을 하자.

이번에는 `flag` `passcod` `passcode.c`가 존재하고있다.
```
passcode@ubuntu:~$ ls
flag  passcode  passcode.c
```

그동안 해왔던대로 `passcode.c`를 열어보자.
```
#include <stdio.h>
#include <stdlib.h>

void login(){
        int passcode1;
        int passcode2;

        printf("enter passcode1 : ");
        scanf("%d", passcode1);
        fflush(stdin);

        // ha! mommy told me that 32bit is vulnerable to bruteforcing :)
        printf("enter passcode2 : ");
        scanf("%d", passcode2);

        printf("checking...\n");
        if(passcode1==338150 && passcode2==13371337){
                printf("Login OK!\n");
                system("/bin/cat flag");
        }
        else{
                printf("Login Failed!\n");
                exit(0);
        }
}

void welcome(){
        char name[100];
        printf("enter you name : ");
        scanf("%100s", name);
        printf("Welcome %s!\n", name);
}

int main(){
        printf("Toddler's Secure Login System 1.0 beta.\n");

        welcome();
        login();

        // something after login...
        printf("Now I can safely trust you that you have credential :)\n");
        return 0;
}
```

그리고 제공된 글에서 컴파일 도중 워닝이 떴다는 것을 언급해주었으므로  
따로 코드를 옮겨와서 컴파일 해준 결과
```
lv0@ubuntu:~$ gcc -o passcode passcode.c
passcode.c: In function ‘login’:
passcode.c:9:17: warning: format ‘%d’ expects argument of type ‘int *’, but argument 2 has type ‘int’ [-Wformat=]
         scanf("%d", passcode1);
                ~^
passcode.c:14:17: warning: format ‘%d’ expects argument of type ‘int *’, but argument 2 has type ‘int’ [-Wformat=]
         scanf("%d", passcode2);
                ~^
```
scanf 사용할때 &를 사용하여 변수의 주소를 불러와야하는데 __코드__ 에서는 변수자체를 입력해서 워닝이 떴음을 알 수 있었다.  
이렇게 변수자체를 입력해주면 변수에 들어있는 값을 주소로 인식을하고 %d로 저장하게된다.


