#6 SQL 인젝션 기초
===================

## SQL 인젝션 – GET/Search

> 사용자가 입력한 값을 검증하지 않고 데이터베이스 쿼리 일부분으로 포함될 때 발생.  
```
공격자 ->  웹 -> DB
      data   sql
```

## 주석처리
```
-- : 한 줄 주석 처리
# : 한줄 주석 처리
/**/ : 다중 주석 처리
```

### Ex)  
```
select * from testtable1 where id="" 일 때 주석을 이용하여  
select * from testtable1 where id="" or 1=1; #";  
이와 같이 처리하면 모든 정보 출력할 수 있음.
````

* `where` 절을 통해 출력을 조절 할 수 있음.

> 검증되지 않은 데이터가 웹에 들어간다면 sql을 변조해서 악용 가능하다.

## 시스템 변수 및 함수
```
database() : DB명을 알려주는 함수  
user() : 현재 사용자의 아이디  
system_user() : 최고 권한 사용자의 아이디  
@@version : DB 서버의 버전  
@@datadir : DB 서버가 존재하는 디렉터리  
```
* `information_schema` : 많은 데이터를 담고있어서 주의가 필요/  
* `concat` : 문자열을 합쳐주는 함수.

## SQL 인젝션 대응방안

> `mysql_real_escape_string()` : 특수문자들에 백슬래시를 붙여 입력 값을 SQL 문법으로 인식되지 않게 방어

## SQL 인젝션 – GET/Select

데이터베이스에서 넘어온 데이터값은 많지만 제한된 갯수만 보여주는 경우가 있다.  
이때 출력을 조절하기 위해 `limit`로 조절해준다.

## 대응방안

prepare stmt. -> 가장 안전.  
미리 SQL 구문을 정해놓고 가정함으로써   
SQL 구문 변조 방지.  

## SQL 인젝션 - POST/Search/Select

위와 완전히 똑같은 구조 단지 파라미터 위치만 다름.
