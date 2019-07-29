Ajax (Asynchronous JavaScript and XML)
=======================
Ajax는 JavaScript를 이용하여 웹브라우저와 웹 서버가 내부적으로 데이터 통신을 하게 한다.  
그리고 변경된 결과를 웹페이지에 프로그래밍적으로 반영함으로써   
웹페이지의 로딩 없이 서비스를 사용 할 수 있게 한다.  
즉, JavaScript를 이용해서 비동기적으로 서버와 브라우저가 데이터를 주고받는 방식을 의미한다.  
이때 사용하는 API가 **XMLHttpRequest** 이다.

# 1. 실습
## 1.1. time.php
```
<?php
$d1 = new DateTime;
$d1->setTimezone(new DateTimezone("asia/seoul"));
echo $d1->format('H:i:s');
?>
```
현재 시간을 출력하는 php 파일
## 1.2. demo1.html
```
<p>time : <span id="time"></span></p>
<input type="button" id="execute" value="execute" />
<script>
document.querySelector('input').addEventListener('click', function(event){
    var xhr = new XMLHttpRequest();
    xhr.open('GET', './time.php');
    xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            document.querySelector('#time').innerHTML = xhr.responseText;
        }
    }
    xhr.send(); 
}); 
</script> 
```
time.php에 접근하여 현재 시간을 페이지에 표시한다.
## 1.3. 해석
```
document.querySelector('input').addEventListener('click', function(event){
```
input 태그를 클릭하면 이벤트 발생

```
var xhr = new XMLHttpRequest();
```
```XMLHttpRequest``` 객체를 생성 하고 참조변수 ```var xhr``` 로 참조 
```
xhr.open('GET', './time.php');
//    'POST/GET' , '파일 경로'
```
```./time.php```경로에 있는 파일을 ```GET```방식으로 연다.
```
 xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            document.querySelector('#time').innerHTML = xhr.responseText;
        }
    }
```
onreadystatechange 이벤트는 서버와의 통신이 끝났을 때 호출되는 이벤트이다.   
xhr이 서버와의 통신이 끝났을 때    
```xhr.readyState === 4``` 인지 그리고  
```xhr.status === 200```인지 확인한다.  
```xhr.readyState === 4``` 는 통신이 완료 되었다는 의미이고    
```xhr.status === 200``` 은 통신이 성공 했음을 의미한다.  
두 조건을 만족한다면 ```id=time```의 하위요소에 값을 추가하는데    
```xhr.responseText```을 innerHTML 형식으로 넣는다(태그 포함)  
```xhr.responseText```는 응답 객체(xhr)의 Text를 의미한다.    
즉 ```id=time```의 하위요소에 time.php의 Text값이 들어간다.  

```
xhr.send(); // 요청 전송
```
이제 서버에 요청을 보내는 것이다.  
여기서 중요한 점은 '비동기 방식' 이라는 것이다.
분명 앞에서 

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
