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

enc 파일의 내용과 IGRUS라는 단어를 이용하여 `print(chr(ord(":") ^ ord('G')))`이 코드를 이용하면   
해결할 수 있음을 알 수 있었다.

```
flag{m0r3_h4rd3r_x0r_3ncrypt10n}
```
