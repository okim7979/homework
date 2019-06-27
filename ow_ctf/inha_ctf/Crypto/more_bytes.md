more_bytes
=========

이번에는 enc파일과 make.py 파일이 같이 들어있다.   
```
enc : /+32($w f!s 1`;*e!t<6!07&dc':

make.py : 

from flag import flag  
from itertools import cycle  

enc = ""  
key = cycle('IGRUS')  

for i,j in zip(flag, key):  
	enc += chr(ord(i) ^ ord(j))  

f = open('enc', 'wt')  
f.write(enc)  
f.close()  

```

enc 파일의 내용과 IGRUS라는 단어를 이용하면 `enc += chr(ord(i) ^ ord(j))` 이 명령어를 통해 플래그를 얻을 수 있을 것이라 유추 할 수 있었다.

`ord(c)는 문자의 아스키 코드값을 리턴하는 함수이다.`  
`chr(i)는 아스키(ASCII) 코드값을 입력으로 받아 그 코드에 해당하는 문자를 출력하는 함수이다.`  


```
flag{m0r3_h4rd3r_x0r_3ncrypt10n}
```
