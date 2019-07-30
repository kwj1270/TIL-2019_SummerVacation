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
# 1. GET 방식
## 1.1. time.php
```
<?php
$d1 = new DateTime;
$d1->setTimezone(new DateTimezone("asia/seoul"));
echo $d1->format('H:i:s');
?>
```
## 1.2. demo.html
```
<p>time : <span id="time"></span></p>
<input type="button" id="execute" value="execute" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#execute').click(function(){
        $.ajax({
            url:'./time.php',
            success:function(data){
                $('#time').append(data);
            }
        })
    })
</script>
```
JQuery는 크로스 브라우징 문제를 알아서 해결해주기에 코드가 일관성이 있으며 
```
var xhr = new XMLHttpRequest()
xhr.open()
```
XMLHttpRequest에 비해서 코드가 훨씬 간결해졌다. 

***
# 2. POST 방식
## 2.1. time2.php
```
<?php
$d1 = new DateTime;
$d1->setTimezone(new DateTimezone($_POST['timezone']));
echo $d1->format($_POST['format']);
?>
```   

## 2.2. demo2.html
```
<p>time : <span id="time"></span></p>
<form>
    <select name="timezone">
        <option value="Asia/Seoul">asia/seoul</option>
        <option value="America/New_York">America/New_York</option>
    </select>
    <select name="format">
        <option value="Y-m-d H:i:s">Y-m-d H:i:s</option>
        <option value="Y-m-d">Y-m-d</option>
    </select>
</form>
<input type="button" id="execute" value="execute" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#execute').click(function(){
        $.ajax({
            url:'./time2.php',
            type:'post',
            data:$('form').serialize(),
            success:function(data){
                $('#time').text(data);
            }
        })
    })
</script>
```   

***
# 3. JSON 처리
> 인용
## 3.1. 소 주제
```
<?php
$timezones = ["Asia/Seoul", "America/New_York"];
echo json_encode($timezones);
?>
```
## 3.1. 소 주제
```
<p id="timezones"></p>
<input type="button" id="execute" value="execute" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#execute').click(function(){
        $.ajax({
            url:'./time3.php',
            dataType:'json',
            success:function(data){
                var str = '';
                for(var name in data){
                    str += '<li>'+data[name]+'</li>';
                }
                $('#timezones').html('<ul>'+str+'</ul>');
            }
        })
    })
</script>
```
