Wargame
=======
## 들어가기전에
lv0 비밀번호 : lv0   
알아야될 기초지식 : gdb 사용법

## lv 0
gdb lv1으로 lv1이 무슨 프로그램인지 확인해본다.   
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
즉 ret부분까지 덮기위해서는 8바이트가 더 필요하므로 260바이트 만큼 쓰레기 값을 담아주고 나머지 4바이트에 원하는 주소값을 넣어준다.


## lv 1

## lv 2

## lv 3
## lv 4
## lv 5
## lv 6
## lv 7
## lv 8
## lv 9
## lv 10
## lv 11
