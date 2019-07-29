Event Type
=======================
> 인용

# 1. 폼
> 'submit'은 폼의 정보를 서버로 전송하는 명령인 submit 동작시에 일어난다.
## 1.1 submit
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

***
## 1.2 change
### 1.2.1 코드

> form 컨트롤의 value 값이 변경 되었을 때 발생하는 이벤트이다.  
> input(text,radio,checkbox), textarea, select 태그에 적용된다.  
> 즉 form의 하위요소인 특정 input과 textarea, select 태그에 적용된다.  

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
### 1.3.1 코드
> focus : 엘리먼트에 포커스가 생겼을 때 발생하는 이벤트   
> blur  : 포커스가 사라졌을 때 발생하는 이벤트   
> <base>, <bdo>, <br>, <head>, <html>, <iframe>, <meta>, <param>, <script>, <style>, <title> 예외

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
***
# 3. 마우스
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```
