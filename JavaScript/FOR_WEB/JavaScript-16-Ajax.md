Ajax (Asynchronous JavaScript and XML)
=======================
Ajax는 JavaScript를 이용하여 웹브라우저와 웹 서버가 내부적으로 데이터 통신을 하게 한다.  
그리고 변경된 결과를 웹페이지에 프로그래밍적으로 반영함으로써   
웹페이지의 로딩 없이 서비스를 사용 할 수 있게 한다.  
즉, JavaScript를 이용해서 비동기적으로 서버와 브라우저가 데이터를 주고받는 방식을 의미한다.  
이때 사용하는 API가 **XMLHttpRequest** 이다.

# 1. GET 방식 (실습)
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
```./time.php```경로에 있는 파일을 ```GET```방식으로 연결시킨다.(전송X)
```
 xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            document.querySelector('#time').innerHTML = xhr.responseText;
        }
    }
```
onreadystatechange 이벤트는 서버와의 통신이 끝났을 때 호출되는 이벤트이다.   
그런데 사실 우리는 서버에 요청을 전송을 하지 않았다.  
하지만 서버와의 통신이 끝났을 때 호출되므로 코드 에러가 나지 않는 것이다. 
  
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
이제 서버에 요청을 전송한다.  
  
실행 결과를 보면 우리는 페이지를 리로딩 할 필요없이 값을 가져온다.

***
# 2. POST 방식 (실습)
## 2.1. time2.php
```
<?php
$d1 = new DateTime;
$d1->setTimezone(new DateTimezone($_POST['timezone']));
echo $d1->format($_POST['format']);
?>
```
현재시간을 츌력하는 php 파일
## 2.2. demo2.html
```
<p>time : <span id="time"></span></p>
<select id="timezone">
    <option value="Asia/Seoul">asia/seoul</option>
    <option value="America/New_York">America/New_York</option>
</select>
<select id="format">
    <option value="Y-m-d H:i:s">Y-m-d H:i:s</option>
    <option value="Y-m-d">Y-m-d</option>
</select>
<input type="button" id="execute" value="execute" />
<script>
document.querySelector('input').addEventListener('click', function(event){
    var xhr = new XMLHttpRequest();
    xhr.open('POST', './time2.php');
    xhr.onreadystatechange = function(){
        document.querySelector('#time').innerHTML = xhr.responseText;
    }
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    var data = '';
    data += 'timezone='+document.getElementById('timezone').value;
    data += '&format='+document.getElementById('format').value;
    xhr.send(data); 
});
</script> 
```
시간대와 시간의 출력 형식을 지정하는 예제이다.
## 2.3. 해석 
```
document.querySelector('input').addEventListener('click', function(event){
```
input 태그를 클릭하면 이벤트 발생
```
    var xhr = new XMLHttpRequest();
    xhr.open('POST', './time2.php');
    xhr.onreadystatechange = function(){
        document.querySelector('#time').innerHTML = xhr.responseText;
    }
```
1의 예제와 비슷하니 자세한 설명은 생략  
기존 'GET' 방식 연결을 'POST' 방식으로 바꾸었다.
```
 xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
```
서버로 전송할 데이터 타입의 형식(MIME)을 지정한다. 
```
var data = '';
    data += 'timezone='+document.getElementById('timezone').value;
    data += '&format='+document.getElementById('format').value;
```
서버로 전송할 데이터 형식에 맞게 만들어야 한다.(이름=값&이름=값...) 
```
  xhr.send(data); 
```
생성된 MIME을 가지고 send()로 서버에 요청을 전송한다.
