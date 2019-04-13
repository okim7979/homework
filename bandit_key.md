Bandit_key
==========

## 0. 기본 설정

bandit.labs.overthewire.org  
2220  
SSH  

putty 종료 안하고 레벨 넘기기 : 
ssh bandit숫자@localhost

무엇인가 파일 만들때 /tmp/danhack 폴더 이용하기

비번 확인하고 싶으면 /etc/bandit_pass/bandit??사용
(이건 자기 레벨만 가능)

## LV. bandit0
알아낸 bandit1비번 :
boJ9jbbUNNfktd78OOpsqOltutMc3MY1

## LV. bandit1
>cat <- ,  
cat ./- ,  
cat <./-

알아낸 bandit2비번 :
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

## LV. bandit2
알아낸 bandit3비번 :
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

## LV. bandit3
알아낸 bandit4비번 :
pIwrPrtPN36QITSp3EQaw936yaFoFgAB

## LV. bandit4
알아낸 bandit5비번 :
koReBOKuIDDepwhWk7jZC0RTdopnAYKh

## LV. bandit5
>find . -type f -size 1033c ! -executable -exec file {} + | grep -w text  
find ./ -size 1033c

>★사이즈 단위  
b:블록단위  
c:바이트 단위(b가 아니다!)  
k:키로 바이트 단위  
w:2바이트 워드  
m:메가 바이트  
g:기가 바이트


알아낸 bandit6비번 :
DXjZPULLxYr17uwoI01bNLQbtFemEgo7

## LV. bandit6
>2>/dev/null  
'Permission denied'와 같은 오류를 무시하기 위해 리다이렉션을 사용할 수 있다.

알아낸 bandit7비번 :
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

## LV. bandit7
>sort 정렬  
uniq 중복 삭제  
wc 지정한 파일이나 파이프라인으로 넘어온 내용의 줄, 단어, 문자 수를 출력해주는 명령어

알아낸 bandit8비번 :
cvX2JJa4CFALtqS87jk27qwqGhBM9plV

## LV. bandit8
>uniq 옵션  
-c : 몇 번 중복되었는지 출력

알아낸 bandit9비번 : 
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

## LV. bandit9
>strings : 바이너리 파일에서 문자열을 추출하는 역할을 한다.  
>* 바이너리 파일 : 바이너리 파일은 기본적으로 "행 단위"가 아닌 모든 파일들을 다 일컫습니다. 
  표시용 문자와 개행문자 외에 다른 기호가 들어 있는 파일들도 바이너리 파일입니다.

알아낸 bandit10비번 : 
truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

## LV. bandit10
>base64  
디코딩시 옵션 -d

알아낸 bandit11비번 :
IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

## LV. bandit11
>Rot13 : https://ko.wikipedia.org/wiki/ROT13  
tr 'a-zA-Z' 'n-za-mN-ZA-M'

알아낸 bandit12비번 :
5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu

## LV. bandit12
>xxd  
xxd -r 16진수 데이터를 바이너리 데이터로 변환  
file을 이용하여 속성 확인  
tar압축, gzip압축 bzip2압축  

알아낸 bandit13비번 :
8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

## LV. bandit13
>ssh 옵션을 통해 bandit14를 접속한다.

알아낸 bandit14비번 :
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e

## LV. bandit14
>nc 명령어를 사용

알아낸 bandit15비번 :
BfMYroe26WYalil77FoDi9qh59eK5xNr

## LV. bandit15
>OPenssl에서 s_client 사용  
의문점 : heartbeating이 무엇이며 -ign_eof이걸 사용 안하고 어캐 비번이 나온것일까....

알아낸 bandit16비번 :
cluFn7wTiGryunymYOu4RcffSxQluehd

## LV. bandit16
>nmap을 사용하여 포트 범위 스캔  
->스캔된 포트들 중 nc 를 이용하여 echo를 이용하여 ssl로만 가능한 포트인지 아닌지 확인  
echo 거부된 포트들만 openssl s_client 사용하여 비번 입력해봄  
포트중 하나 RSA key 뱉어냄  
이걸 /tmp/danhack 폴더에 p.privat으로 만들어줌  
(chmod 600으로 권한 막아줘야지 17입장시 워닝 안뜸)  
p.private로 입장

알아낸 bandit17비번 :
xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn

## LV. bandit17
>diff 사용

알아낸 bandit18비번 :
kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd

## LV. bandit18
>ssh -t 옵션을 사용  
pseude 터미널 할당을 강제한다. 원격 시스템에서 임의의 화면 기반 프로그램을 실행하는 사용된다.

알아낸 bandit19비번 :
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

## LV. bandit19
>setuid뜻 알면 풀기가능.

알아낸 bandit20비번 :
GbKksEFF4yrVs6il55v6gwY5aVje5f0j

## LV. bandit20
>터미널 최소 2개 열어주어야함  
nc 명령어를 이용하여 listen 포트 하나 열어준다  
다른 터미널에서 ./suconnect를 이용해서 방금 연 포트에 접속  
포트 연쪽에서 20 비번 입력해주면 답 출력  

알아낸 bandit21비번 :
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr

## LV. bandit21
>cron과 crontab의 개념을 알아본 후  
/etc/cron.d/에 있는 파일이 무슨일을 하는지 확인

알아낸 bandit22비번 :
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI

## LV. bandit22
>.sh파일 열고 해석해보자

알아낸 bandit23비번 :
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n

## LV. bandit23
>스크립트 직접 만들어서 적용  
chmod -R 777  
-R : 하위 디렉토리의 모든 권한을 변경한다  
꼭 적용해줘야함!!!

알아낸 bandit24비번 :
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ

## LV. bandit24
>스크립트를 이용한 brute-forcing 적용

알아낸 bandit25비번 :
uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG

---  
여기서부터는 비번 입력해도 접속이 안된다...

## LV. bandit25
>/etc/passwd 살펴보기  
화면창을 줄여서 more명령어의 허점 이용한다!  
more 명령어 v입력시 vi 모드 진입   
:r php.ini php.ini의 내용을 현재 커서가 있는데로 불러온다. <- 이걸 이용해서 :r /etc/bandit_pass/bandit26  
하지만 쉘을 얻지 못 했기 때문에 접속 불가능  
:set shell=/bin/bash  
:shell
로 접속해준다.

알아낸 bandit26비번 :
5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z

## LV. bandit26
>25단계에서 접속한 상태에서 bandit27-do 가 무슨 파일인지 확인  
bandit27-do실행

알아낸 bandit27비번 :
3ba3118a22e93127a4ed485be72ef5ea

---

## LV. bandit27
>git clone 이용하여 비번 획득

알아낸 bandit28비번 :
0ef186ac70e04ea33b4c1853d2526fa2

## LV. bandit28
>git log를 통해 무엇이 바뀌었는지 확인한다.  
git diff를 통해 차이점을 확인한다.

알아낸 bandit29비번 :
bbc96594b4e001778eee9975372716b2

## LV. bandit29
>git 브랜치 목록 확인하고   
이동하면서 로그 확인

알아낸 bandit30비번 :
5b90576bedb2cc04c86a9e924ce42faf

## LV. bandit30
>git tag와 관련된 문제

알아낸 bandit31비번 :
47e603bb428404d265f59c42920d81e5

## LV. bandit31
>.gitignore가 뭔지 알아야함  
add -> commit -> push

알아낸 bandit32비번 :
56a9bf19c63d650ce78e6ec0354ee45e

## LV. bandit32
>$0 사용하면 정상적으로 출력된다  
한번 사용자 명을 조회해보자  
bandit33일 것이다.

알아낸 bandit33비번 :
c9c3199ddf4121b10cf581a98d51caee

**END**
