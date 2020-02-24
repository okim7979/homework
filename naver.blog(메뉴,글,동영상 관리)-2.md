# 동영상

* 입력칸

![](https://blogfiles.pstatic.net/MjAyMDAyMjRfMiAg/MDAxNTgyNTM1MzMzOTI1.ILVrN_wThn5LzRgzaDd91XxLX3lg9GE89ByQlaHjLOMg.bUqqbK7ojGdTLApqV1pccqREtpZ6DYG0jqF6pDXlq2Qg.JPEG.rlaeoghks823/K-098.jpg)

동영상을 검색할 수 있는 입력창이 있다.  

여기서 취약점을 찾아보기 위해 여러 특수문자를 입력해보던 도중,

![](https://blogfiles.pstatic.net/MjAyMDAyMjRfMjc3/MDAxNTgyNTM1MzMzOTE0.7qRlxsD0Zbf0TPJHAsa1cU907w9x1IAbPebcLgZZi7Ag.HMOLkZmkIISY53ZBnWzUiOIeZjCqro7mfVol74by7Ecg.JPEG.rlaeoghks823/K-097.jpg)

"> 로 `input`칸을 문구를 닫아 버리면 위와 같이 그 뒤에 코드가 그대로 출력이 됨을 확인 할 수 있었다.

그 뒤에 어떤 문구가 오든 그냥 그대로 글자로 출력이 되는 것을 보면  

이러한 방식을 통해 ">으로 문장을 끝내고 어택코드를 넣는 방식으로 통해 발생하는 취약점을 방지하는 것으로 보인다.


![](https://blogfiles.pstatic.net/MjAyMDAyMjRfNDEg/MDAxNTgyNTM1MzMzOTI3.lYZVub66QqqqejdWTW-VlasoRL7mWnJM7Cz_9VJKljwg.BvwGN_2ka6iLtp5i0ZAkKRS3P3j4Y_nCyQiu-8BOthQg.JPEG.rlaeoghks823/K-099.jpg)

위는 정상적인 입력값을 넣었을때 출력되는 화면이다.
