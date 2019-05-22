# [Toddler's Bottle] random
random에서 제공하는 글을 확인해보자.
```
Daddy, teach me how to use random value in programming!

ssh random@pwnable.kr -p2222 (pw:guest)
```
putty로 접속해주자.

```
random@ubuntu:~$ ls
flag  random  random.c
```

```
random@ubuntu:~$ cat random.c
#include <stdio.h>

int main(){
        unsigned int random;
        random = rand();        // random value!

        unsigned int key=0;
        scanf("%d", &key);

        if( (key ^ random) == 0xdeadbeef ){
                printf("Good!\n");
                system("/bin/cat flag");
                return 0;
        }

        printf("Wrong, maybe you should try 2^32 cases.\n");
        return 0;
}
```
이번코드는 랜덤함수를 이용하여 입력된 key와 xor를 시킨다.
그러나 랜덤함수는 시드값을 바꾸어주지 않는 이상 계속 같은 값을 갖게 된다.

따라서 gdb를 통해 랜덤값이 무엇인지 확인해보자.
```
0x0000000000400609 in main ()
(gdb) x/x $rbp - 0x4
0x7ffd8db75b5c: 0x6b8b4567
```
랜덤값은 0x6b8b4567가 된다는 것을 알 수 있다.

xor의 원리에 따라서  

key = a | ^ | random = b | = | 0xdeadbeef
:--------:|---|:------------:|---|:-----------:
a |^| b| =| c
b| ^| c| =| a
rand |^| deadbeef| =| key


랜덤값과 `0xdeadbeef`를 xor시켜준 값은
```
random@ubuntu:~$ python
Python 2.7.12 (default, Nov 12 2018, 14:36:49)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 0x6b8b4567 ^ 0xdeadbeef
3039230856
>>>
```
`3039230856`임을 확인할 수 있다.

```
random@ubuntu:~$ ./random
3039230856
Good!
Mommy, I thought libc random is unpredictable...
```
