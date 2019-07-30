JSON(JavaScript Object Notation)
=======================
JavaScript 에서 객체를 만들 때 사용하는 표현식이며  
사람도 이해하기 쉽고 기계도 이해하기 쉬우며 데이터의 용량이 적다.  
서로 다른 언어들끼리 그 자료형태를 유지한채로 테이터를 주고 받는 용도로 사용된다.  
배열 -> 배열 / 객체 -> 객체

# 1. JSON API
> ECMAscript 5에는 JSON을 공식적으로 지원하는 API가 포함되었다. 
## 1.1. JSON.parse()
문자열 -> JSON 객체
```
var infoObj = JSON.parse(info);
```
문자열 ```info```를 JSON 객체화하여  
```infoObj```에게 참조 시켰다.  
## 1.2. JSON.stringify()
```
var infostr = JSON.stringify(infoObj);
```
객체 ```infoObj```를 문자열화 하여  
```infostr```에게 참조 시켰다.

***
# 2. Ajax 와 JSON
> JSON의 진가는 서버와 통신을 할 때 드러난다.

## 2.1. JSON 미사용 코드
### 2.1.1. time.php
```
<?php
$timezones = ["Asia/Seoul", "America/New_York"];
echo implode(',', $timezones);
?>
``` 
서버 쪽에서는 타임라인의 리스트를 콤마로 구분해서 전달한다.  
### 2.1.2. time.php 결과
```
Asia/Seoul,America/New_York
```
클라이언트 측에서는 이를 받아서 처리한다.
### 2.1.3. demo.html
```
<p id="timezones"></p>
<input type="button" id="execute" value="execute" />
<script>
document.querySelector('input').addEventListener('click', function(event){
    var xhr = new XMLHttpRequest();
    xhr.open('GET', './time.php');
    xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            var _tzs = xhr.responseText;
            var tzs = _tzs.split(',');
            var _str = '';
            for(var i = 0; i< tzs.length; i++){
                _str += '<li>'+tzs[i]+'</li>';
            }
            _str = '<ul>'+_str+'</ul>';
            document.querySelector('#timezones').innerHTML = _str;
        }
    }
    xhr.send(); 
}); 
</script>
```
## 2.2. JSON 사용 코드
### 2.2.1. time2.php
```
<?php
$timezones = ["Asia/Seoul", "America/New_York"];
header('Content-Type: application/json');
echo json_encode($timezones);
?>
```
### 2.2.2. time2.php 결과
```
["Asia\/Seoul","America\/New_York"]
```
### 2.2.3. demo2.html
```
<p id="timezones"></p>
<input type="button" id="execute" value="execute" />
<script>
document.querySelector('input').addEventListener('click', function(event){
    var xhr = new XMLHttpRequest();
    xhr.open('GET', './time2.php');
    xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            var _tzs = xhr.responseText;
            var tzs = JSON.parse(_tzs);
            var _str = '';
            for(var i = 0; i< tzs.length; i++){
                _str += '<li>'+tzs[i]+'</li>';
            }
            _str = '<ul>'+_str+'</ul>';
            document.querySelector('#timezones').innerHTML = _str;
        }
    }
    xhr.send(); 
}); 
</script> 
```
```
var tzs = JSON.parse(_tzs);
```
***
# 3. 서버로 데이터 전송
> 서버로 JSON 데이터를 전송하는 것도 가능하다. 
## 3.1. time3.php
```
<?php
$data = json_decode(file_get_contents('php://input'), true);
$d1 = new DateTime;
$d1->setTimezone(new DateTimezone($data['timezone']));
echo $d1->format($data['format']);
?>
```
## 3.2. demo4.html
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
    xhr.open('POST', './time3.php');
    xhr.onreadystatechange = function(){
        document.querySelector('#time').innerHTML = xhr.responseText;
    }
    var data = new Object();
    data.timezone = document.getElementById('timezone').value;
    data.format = document.getElementById('format').value;
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.send(JSON.stringify(data)); 
});
</script>
```
