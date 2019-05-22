# [Toddler's Bottle] flag
flag에서 제공하는 글을 확인해보자.
```
Papa brought me a packed present! let's open it.

Download : http://pwnable.kr/bin/flag

This is reversing task. all you need is binary
```
이번은 리버싱 문제이며 바이너리파일을 하나 제공해주었다.   
아이다를 통해 flag파일을 확인해본 결과 upx로 패킹된 파일임을 확인 할 수 있었다.
```
LOAD:000000000044A695	00000010	C	//upx.sf.net $\n
```

그래서 다음과같이 upx언패킹 툴을 이용하여 언패킹 해주었다.
```
Z:\작업\upx\upx>upx.exe -d flag
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2013
UPX 3.91w       Markus Oberhumer, Laszlo Molnar & John Reiser   Sep 30th 2013

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
    887219 <-    335288   37.79%  linux/ElfAMD   flag

Unpacked 1 file.
```
그후 아이다를 통해 다시 flag 파일을 열어준 결과
```
.rodata:0000000000496628	0000002A	C	UPX...? sounds like a delivery service :)
```
flag를 얻을 수 있었다.
