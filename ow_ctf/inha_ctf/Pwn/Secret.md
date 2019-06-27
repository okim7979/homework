Secret
======

처음에 Secret이라는 실행파일을 얻을 수 있는데 이 실행파일은 아래와 같이 되어있다.
```
Dump of assembler code for function main:
   0x00000000004006f6 <+0>:	push   rbp
   0x00000000004006f7 <+1>:	mov    rbp,rsp
   0x00000000004006fa <+4>:	sub    rsp,0x100
   0x0000000000400701 <+11>:	mov    rax,QWORD PTR [rip+0x200960]        # 0x601068 <stdout@@GLIBC_2.2.5>
   0x0000000000400708 <+18>:	mov    esi,0x0
   0x000000000040070d <+23>:	mov    rdi,rax
   0x0000000000400710 <+26>:	call   0x400590 <setbuf@plt>
   0x0000000000400715 <+31>:	mov    edi,0x400814
   0x000000000040071a <+36>:	mov    eax,0x0
   0x000000000040071f <+41>:	call   0x4005b0 <printf@plt>
   0x0000000000400724 <+46>:	lea    rax,[rbp-0x100]
   0x000000000040072b <+53>:	mov    rsi,rax
   0x000000000040072e <+56>:	mov    edi,0x40082b
   0x0000000000400733 <+61>:	mov    eax,0x0
   0x0000000000400738 <+66>:	call   0x4005e0 <__isoc99_scanf@plt>
   0x000000000040073d <+71>:	lea    rax,[rbp-0x100]
   0x0000000000400744 <+78>:	mov    esi,0x601060
   0x0000000000400749 <+83>:	mov    rdi,rax
=> 0x000000000040074c <+86>:	call   0x4005d0 <strcmp@plt>
   0x0000000000400751 <+91>:	test   eax,eax
   0x0000000000400753 <+93>:	jne    0x400770 <main+122>
   0x0000000000400755 <+95>:	mov    edi,0x400831
   0x000000000040075a <+100>:	call   0x400580 <puts@plt>
   0x000000000040075f <+105>:	mov    edi,0x40084a
   0x0000000000400764 <+110>:	mov    eax,0x0
   0x0000000000400769 <+115>:	call   0x4005a0 <system@plt>
   0x000000000040076e <+120>:	jmp    0x40077a <main+132>
   0x0000000000400770 <+122>:	mov    edi,0x400857
   0x0000000000400775 <+127>:	call   0x400580 <puts@plt>
   0x000000000040077a <+132>:	mov    eax,0x0
   0x000000000040077f <+137>:	leave  
   0x0000000000400780 <+138>:	ret    
End of assembler dump.
```

gdb를 이용하여 어떤식으로 프로그램이 작동이 되나 살펴보는 도중 아래와 같은 단서를 찾아낼 수 있었다.
```
0x40074c <main+86>     call   strcmp@plt <0x4005d0>
        s1: 0x7fffffffe040 ◂— 0x7ffff7006161 /* 'aa' */
        s2: 0x601060 (secret) ◂— 0xdeadbeef
```

비밀번호는 `deadbeef`이며 16진수로 이루어져 있었고 이것을 입력해줄때도 16진수로 입력해야한다.

```
root@kali:~# (python -c 'print "\xef\xbe\xad\xde"';cat)|nc igrus.cf 8101
Give me secret code > Good job!, Here is flag.
flag{c4n_y0u_s3nd_n0n_pr1nt4bl3_c0d3?}
```
