#9 Blind SQL 인젝션
==================

## Blind SQL 인젝션

쿼리의 결과를 참과 거짓만으로만 출력하는 페이지에서 사용하는 공격  
출력 내용이 참과 거짓 밖에 없어서 데이터베이스의 내용을 추측하여 쿼리를 조작.

## 공격 플로우

1. SQL의 결과를 데이터베이스가 참과 거짓으로 주는가?
2. 원한느 데이터의 길이 확인
3. 원하는 데이터의 스트링 확인

### 기초 공격 구문

`ascii(substr((select table name from information schema.tables where table_type='base table' limit 0,1),1,1));`

간단한 스크립트를 통한 자동화가 필요

## 사용되는 함수
```
substr(string, 시작위치, 길이) : 여러 문자열중 한개만 불러옴 limit랑 사용 형식 비슷  
substring(string, 시작위치, 길이)  
length(string) : 길이를 가져옴  
right(string, length), left(string, length) : 
 ```
## SQL MAP 인젝션 Points 설정

* B : Boolean-based blind SQL injection
* E : Error-based SQL injection
* U : UNION query SQL injection
* S : Stacked queries SQL injection
* T : Time-based blind SQL injection
