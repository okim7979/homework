#7 SQL 인젝션(SQLMAP & Metasploit 활용)
======================================

## SQL 인젝션(SQLMAP & Metasploit 활용) 

blog.naver.com/isc0304

## SQLMAP
> sql 인젝션 결함을 사용한 익스플로잇, 데이터베이스 서버를 접수를 자동화한 오픈 소스 침투 점검 도구

IFNULL(NULL아니면,NULL이면)

sqlmap -h = 명령어 확인 가능.

sleep : 설정된 시간동안 DB가 잠을 잔다.

## Metasploit
```
https://github.com/naiduv/codecrawl/blob/master/code/bwapp-sqli.rb

wget https://raw.githubusercontent.com/naiduv/codecrawl/master/code/bwapp-sqli.rb  
cp bwapp-sqli.rb /usr/share/metasploit-framework/modules/exploits/multi/http/bwapp-cmdi.rb  
msfdb init  
msfconsole  
````

위와같은 방법으로 모의해킹 진행.

* `meterpreter` : 다른 컴퓨터 원격 조정 가능한 쉘
