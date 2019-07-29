Event Type
=======================

# 1. 폼
## 1.1 submit
> 'submit'은 폼의 정보를 서버로 전송하는 명령인 submit 동작시에 일어난다.

### 1.1.1 코드

```
<form id="target" action="result.html">
    <label for="name">name</label> <input id="name" type="name" />
    <input type="submit" />
</form>
<script>
var t = document.getElementById('target');
t.addEventListener('submit', function(event){
    if(document.getElementById('name').value.length === 0){
        alert('Name 필드의 값이 누락 되었습니다');
        event.preventDefault();
    }
});
</script>
```
submit 명령이 실행 되면 function을 실행한다.  
해당 예제는 텍스트필드의 값이 0일 경우 경고창을 띄우고  
기본 동작인 전송(submit)을 취소한다.  

## 1.2 change
> form 컨트롤의 value 값이 변경 되었을 때 발생하는 이벤트이다.  
> input(text,radio,checkbox), textarea, select 태그에 적용된다.  
> 즉 form의 하위요소인 특정 input과 textarea, select 태그에 적용된다.  

### 1.2.1 코드


```
<p id="result"></p>
<input id="target" type="name" />
<script>
var t = document.getElementById('target');
t.addEventListener('change', function(event){
    document.getElementById('result').innerHTML=event.target.value;
});
</script>
```
name 즉 텍스트 필드의 값이 바뀌면 ```<p id="result"></p>```에 값을 넣어준다.(innerHTML)  

## 1.3 blur , focus
> focus : 엘리먼트에 포커스가 생겼을 때 발생하는 이벤트   
> blur  : 포커스가 사라졌을 때 발생하는 이벤트   

```<base>, <bdo>, <br>, <head>, <html>, <iframe>, <meta>, <param>, <script>, <style>, <title>```
는 적용이 불가능 하다.

### 1.3.1 코드

```
<input id="target" type="name" />
<script>
var t = document.getElementById('target');
t.addEventListener('blur', function(event){
    alert('blur');  
});
t.addEventListener('focus', function(event){
    alert('focus'); 
});
</script>
```
input 요소에 focus(커서)가 잡히면 focus 이벤트 실행이고 (```alert('focus');```)   
focus가 벗어나면 즉 blur가 되면 blur 이벤트 실행이다.(```alert('blur');```)  

***
# 2. 문서 로딩
> 웹페이지를 프로그래밍적으로 제어하기 위해서는 웹페이지의 모든 요소에 대한 처리가 끝나야 한다.  
> 이것을 알려주는 이벤트가 **load, DOMContentLoaded**이다.  

## 2.1. 코드 비교
### 2.1.1. 잘못된 코드
```
<html>
    <head>
        <script>
        var t = document.getElementById('target');
        console.log(t);
        </script>
    </head>
    <body>
        <p id="target">Hello</p>
    </body>
</html>
```
```<p id="target">Hello</p>```가 아직 정의되어 있지 않은데   
```var t = document.getElementById('target');```를 호출 했으므로 실행 결과는 null 이다.
이와 같은 문제를 해결하려면  문서 끝에 코드를 위치시키는 것이다.(```</body>```위에)
### 2.1.2. 문서 끝에 코드 (추천)
```
<html>
    <head>
    </head>
    <body>
        <p id="target">Hello</p>
        <script>
            var t = document.getElementById('target');
            console.log(t);
        </script>
    </body>
</html>
```
<head>태그 안쪽이 아닌 <body>태그의 끝 부분에 위치 시켰다. 
이는 문서의 HTML 요소를 먼저 보여줌으로써 페이지의 로딩시간이 비교적 적어보인다.
그래서 많은 프로그래머들이 이러한 방식을 사용한다.  
그렇다면 <head>에 위치 시킬수 없는것인가? 물론 방법은 있다.  

### 2.1.3. load 이벤트
```
<head>
    <script>
        window.addEventListener('load', function(){
            var t = document.getElementById('target');
            console.log(t);
        })
    </script>
</head>
<body>
    <p id="target">Hello</p>
</body>
```
load 이벤트는 문서내의 모든 리소스(이미지, 스크립트)의 다운로드가 끝난 후에 실행된다.  
그러나 에플리케이션의 구동이 너무 지연되는 부작용을 초래할 수 있다.
즉, 모든 다운로드가 끝나면 실행 되므로 비교적 느려보인다.
그래도 그나마 타협점이 하나 있다.

### 2.1.4. DOMContentLoaded
```
<html>
    <head>
        <script>
            window.addEventListener('load', function(){
                console.log('load');
            })
            window.addEventListener('DOMContentLoaded', function(){
                console.log('DOMContentLoaded');
            })
        </script>
    </head>
    <body>
        <p id="target">Hello</p>
    </body>
</html>
```
DOMContentLoaded는 문서에서 스크립트 작업을 할 수 있을 때 실행되기 때문에 이미지 다운로드를 기다릴 필요가 없다.  
즉 일반적인 DOM 데이터만 로딩이 완료되면 실행 한다.
