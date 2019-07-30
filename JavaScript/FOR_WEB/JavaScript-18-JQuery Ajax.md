JQuery Ajax
=======================
라이브러리는 여러가지 편리한 기능들을 제공해준다. 
JQuery도 마찬가지이다. 예를 들면 크로스브라우징의 문제를 해결해주기도 한다.

JQuery는 Ajax와 관련해서 많은 API를 제공한다.
그 중 가장 중요한 API는 ```$.ajax()```메소드이다.    
  
**문법**  
```jQuery.ajax( [settings ] )```
```
data     :  서버로 데이터를 전송할 때 이 옵션을 사용한다.  

dataType :  서버측에서 전송한 데이터를 어떤 형식의 데이터로 해석할 것인가를 지정한다.   
            값으로 올 수 있는 것은 xml, json, script, html이다. 형식을 지정하지 않으면 jQuery가 알아서 판단한다.

success  :  성공했을 때 호출할 콜백을 지정한다.
            Function( PlainObject data, String textStatus, jqXHR jqXHR )

type     :  데이터를 전송하는 방법을 지정한다. get, post를 사용할 수 있다.
```
# 1. $.GET 방식
## 1.1. time.php
```

```
## 1.2. demo.html
### 1.2.1. 내용1
```
내용1
```

***
# 2. POST 방식
> 인용
## 2.1. 소 주제
### 2.1.1. 내용1
```
내용1
```   

***
# 3. JSON 처리
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```
