DOM_JQuery
=======================
> JQuery는 DOM을 내부에 감추고 보다 쉽게 웹페이지를 조작 할 수 있도록 돕는 도구 (라이브러리)  
  
### 사용법  
**구조**
```
$('선택자').메소드('속성', '값');
$('선택자').메소드({'프로퍼티' : '값'});
```
* **기본**
```
    JQuery(document).ready(function($){
      $('선택자').메소드();
})
```
* **요약(간략)**   
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

## 1.1. DOM 과 JQuery 비교
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
JQuery 함수의 반환값이 JQuery객체이기 때문에 연속으로 메소드를 작성 할 수 있다.
```
$('li').css("color", "red").css("background-color" , "blue");
```
체이닝 대신에 객체로 표현하면 한번에 선언 할 수 있다.
```
$('li').css(
            {"color" : "red",
            {"background-color" , "blue"},
            );
```
## 2.3. 유사배열
JQuery는 가져온 대상을 유사배열 형태로 저장한다.   
즉, Index를 통해 우리는 부분적인 제어를 할 수 있다.  
단, 변수[index]의 값은 DOM 객체이다.  
따라서 JQuery의 기능을 이용해서 이 객체를 제어하려면 JQuery함수를 이용해야 한다.  
```
li[0].css('color':'red');     -> X
$(li[0]).css('color':'red');  -> O
```
## 2.4. 쿼리문과 제이쿼리 객체
$()는 제이쿼리 객체를 반환 해주는 메소드이다.
앞서 2.3 유사 배열에 나온 예제를 다시 설명해 보면 아래와 같다.
```
li[0].css('color':'red');     -> 일반 객체 -> 제이쿼리 객체가 아니다 ->  X
$(li[0]).css('color':'red');  -> 일반 객체를 제이쿼리화             -> O
```
즉 제이쿼리 객체만 제이쿼리 메소드를 이용 할 수 있는 것 이다.  
this 와 $(this)도 마찬가지이다.  
제이쿼리에서 this를 제어하기 위해서는 $(this)로 사용하는 것이다.  
## 2.5. Map
map은 JQuery 객체의 엘리먼트를 하나씩 순회한다.  
이 때 첫번째 인자로 전달된 함수가 호출되는데  
처번째 인자로 엘리먼트의 인덱스(현재요소의 Index)  
두번째 인자로 엘리먼트 객체(DOM)가 전달된다.  
(주의점은 웬만한 배열관련 function들은 (요소 , 인덱스 , 배열)이런식으로 인자가 들어온다.)  
```
var li = $('li');
li.map(function(index , elem){
  console.log(index.elem);
  $(elem).css({'color':'red'});     //elem은 DOM 객체이므로 쿼리함수로 감싸준다.
});
```

***
# 3. JQuery API (속성)
## 3.1. 속성 메소드
JQuery를 이용해서 속성을 제어할 수 있다.
```
.attr('속성');               // 속성 값 얻기
.attr('속성', '변경값');     // 속성 값 변경
.removeAttr('속성');        // 속성 삭제하기 
  
var t = $('#target');                               // 선택자를 통한 id가 target인 요소를 제이쿼리 객체로 얻는다.
t.attr("href");                                     // 대상의 href 속성 값 얻기
t.attr("title", "https://github.com/kwj1270");      // 대상의 title 속성 값을 변경한다.
t.removeAttr("title");                              // 대상의 title 속성을 없앤다.
```
## 3.2. 속성과 프로퍼티
DOM과 마찬가지로 JQuey도 속성과 프로퍼티를 구분한다.
```
속성      : .attr()
프로퍼티   : .prop()   

<a id="t1" href="./demo.html">opentutorials</a>
<input id="t2" type="checkbox" checked="checked" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
var t1 = $('#t1');
console.log(t1.attr('href'));       // 상대 경로 
console.log(t1.prop('href'));       // 절대 경로
 
var t2 = $('#t2');
console.log(t2.attr('checked'));    // checked / unchecked
console.log(t2.prop('checked'));    // true / false
</script>
```
또한 JQuery를 이용하면 프로퍼티의 이름으로 어떤 것을 사용하건 올바른 것으로 교정해준다
```
<div id="t1">opentutorials</div>
<div id="t2">opentutorials</div>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
$('#t1').prop('className', 'important'); 
$('#t2').prop('class', 'current');            -> 프로퍼티는 원래 className 이다
</script>
```

***
# 4. JQuery 조회범위 제한
## 4.1. Selector context(선택자)
조회범위를 제한하는 것, 그 제한된 범위를 Selector Context라고 한다. (선택자)
```
<ul>
    <li class="marked">html</li>
    <li>css</li>
    <li id="active">JavaScript
        <ul>
            <li>JavaScript Core</li>
            <li class="marked">DOM</li>
            <li class="marked">BOM</li>
        </ul>
    </li>
</ul>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $( ".marked", "#active").css( "background-color", "red" );
    //또는
    $( "#active .marked").css( "background-color", "red" );
</script>
```
## 4.2. find()
 .find()는 JQuery 객체 하위객체 내에서 조회하는 기능을 제공한다.
 ```
 $( "#active").find('.marked').css( "background-color", "red" );
 하위 객체 중에서 .marked를 찾는다.
 
 $('#active').css('color','blue').find('.marked').css( "background-color", "red" );
 물론 이렇게 사용도 가능하다.
 ```
 단 find를 너무 복잡하게 사용할 경우 유지보수하기가 어려워진다.
