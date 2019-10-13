#3 HTML 인젝션
=============

## 인젝션 
> 코드들에다가 다른 코드를 주입하여 코드의 흐름을 변화시킴.

## HTML 
> 문서를 보여주는 언어.

## HTML 인젝션 - Reflected(GET/POST)
> 문서에다가 다른 코드를 넣음.

> 취약한 매개변수에 악의적인 HTML 코드를 삽입하는 공격.

> 클라이언트(html<javascript>)와 서버(php, jsp, asp...)가 통신 할 때 서로 사용하는 언어가 다름.

### Reflected 
> 클라이언트 자신에게 반응시켜서 반사라고함.

``` 
127.0.0.1 8080
나 자신에게 프록시
8080 : burpsuite에서 8080가 열려있다고 설정되어있음. 
```

`<script>alert("Test")</script>`

### cd /var/www/bWAPP/ 
> 웹페이지 소스가 있는 곳.

* gedit 사용중 다른 작업 하고싶을때 ctl+z 누른후 bg 명령어 입력

## xss_check_1
> <과 >를 &lt와 &gt로 치환하여 취약점 방지함.
> #### 문제점 
> 더블 인코딩해서 넘기면 다시 취약점이 발생한다.

* https://meyerweb.com/eric/tools/dencoder/ 에서 더블 인코딩 가능.

## xss_check_3
> htmlspecialchars를 HTML entities로 다 바꿔준다.  
또한 $input = urldecode($input)도 제거해줘서 취약점 제거.  
테그를 근본적으로 막고있음.  

## get요청과 post요청의 차이점
> 헤더와 바디의 파라미터 넣어주는 변수들이 어디있는지에 대한 차이. 
 
> get은 헤더를 통해 데이터 전달.  
> post는 바디를 통해 데이터 전달.   

> length로 길이를 명시해줌.  

## HTML 인젝션 - Stored(Blog)
> 악의적인 데이터를 서버에 올려서  
실행자가 그 데이터를 봄으로써 문제가 발생한다.  
파급력이 크다.  
