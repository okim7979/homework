#14 XSS(Reflected)
==================

## XSS - Reflected

취약한 매개변수에 악의적인 HTML 코드를 삽입하는 공격  
HTML 태그로 악의적인 사이트에 연결하거나 악성 파일을 다운로드 하도록 유도

## AJAX, JSON
헤더를 만들어서 content 타입을 명시해서 스크립트를 실행하지 않고 데이터를 넘겨주기만함. 

* 이를 우회하기 위해 img 태그를 활용 
`<img src=x onerror='alert(1)'>`

## phpmyadmin

CVE-2010-4480 
error.php 에서 문제 발생  
BBcode에서 XSS 공격 가능.

phpself
서버변수

* `onmouseover` : 마우스를 올려 놓으면 동작하는 함수.
