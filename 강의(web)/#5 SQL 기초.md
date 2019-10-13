#5 SQL 기초
===========

`접속 : mysql -u root -p`

## SQL 기본 문법

* 표의 형식 확인 `desc`

* 조회 `select * from 이름 ` 

* 선택 조회 `select * from 이름 where 조건  `
(비교, 논리, 범위, 집합, 패턴)

* 검색 레코드 수 제한 `select * from 이름 limit 1` : 레코드 1개 출력  
		"		  `limit 0,2` : 첫 번째 레코드(0)부터 2개의 레코드를 가져옴
    
* `select *from 이름1 union select * from 이름2` 두개 테이블 합침. (이름1 부분을 거짓하고 이름2 부분이 참이면 이름2부분만 출력)
```
-> 앞의 값을 거짓으로 만들어버리고 뒤에 새로운 값을 넣어 데이터 빼오기 가능.
```

* 값 넣을 때 : insert
