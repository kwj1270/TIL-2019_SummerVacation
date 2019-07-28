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
# 2. JQuery 객체
> JQuery 함수의 리턴값으로 JQuery 함수를 이용해서  
> 선택한 엘리먼트들에 대해서 처리할 작업을 프로퍼티로 가지고 있는 객체 
## 2.1. 암시적 반복
jQuery 객체의 가장 큰 특징은 암시적인 반복을 수행한다는 점이다.
DOM과 다르게 선택된 엘리먼트 전체에 대해서 동시에 작업이 처리된다.
```
var list = $('li').css("color", "red").css("background-color" , "blue");

모든 <li>의 color가 red가 되었다.
```
JQuery가 아닌 일반 JS 경우 반복문을 사용해야 된다.

## 2.2. 체이닝
```
$('li').css("color", "red").css("background-color" , "blue");
```
JQuery 함수의 반환값이 JQuery객체이기 때문에 연속으로 메소드를 작성 할 수 있다.
```
$('li').css(
            {"color" : "red",
            {"background-color" , "blue"},
            );
```
체이닝 대신에 객체로 표현하면 한번에 선언 할 수 있다.
## 2.3. 유사배열

## 2.4. Map

***
# 3. JQuery API
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
# 4. JQuery 조회범위 제한
