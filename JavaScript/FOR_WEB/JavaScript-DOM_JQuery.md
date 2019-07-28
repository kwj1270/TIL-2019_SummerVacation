DOM_JQuery
=======================
> JQuery는 DOM을 내부에 감추고 보다 쉽게 웹페이지를 조작 할 수 있도록 돕는 도구 (라이브러리)  
  
### 사용법  
구조
```
$('선택자').메소드('속성', '값');
$('선택자').메소드({'프로퍼티' : '값'});
```
기본
```
    JQuery(document).ready(function($){
      $('선택자').메소드();
})
```
요약(간략)   
```
   $function(){
      $('선택자').메소드();
})
```
기본 버전은 이벤트를 이용한 방법으로 HTML 문서의 준비가 끝나면 실행된다.

# 1. 제어대상 찾기
JQuery 함수는 인자를 전달하면 **JQery객체**를 리턴한다.
이객체는 선택자에 해당하는 엘리먼트를 제어하는 다양한 메소드를 가지고 있다.
HTMLCollection과 마찬가지로 여러 요소가 존재 할 때는 유사 배열에 담아서 반환한다.

## 1.1 DOM 과 JQuery 비교
### DOM
```
var lis = document.getElementsByTagName('li');
for(var i=0; i&lt;lis.length; i++){
    lis[i].style.color='red'; 
```
### JQuery
```
$function(){
  $('.active').css('color', 'red');
}
```
코드에서 느껴지 듯이 JQuery가 단순 JS보다 더 간략하다.
***
# 2. 대주제
> 인용
## 2.1. 소 주제
### 2.1.1. 내용1
```
내용1
```   

***
# 3. 대주제
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```


```
예시)
jQuery( document ).ready(function( $ ) {
  $('body').prepend('<h1>Hello world</h1>');
});
```

