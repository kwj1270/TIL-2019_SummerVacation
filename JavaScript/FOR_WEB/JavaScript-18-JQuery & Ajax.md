JQuery & Ajax
=======================
라이브러리는 여러가지 편리한 기능들을 제공해준다.   
JQuery도 마찬가지이다.   
예를 들면 크로스브라우징의 문제를 해결해주기도 한다.  
  
JQuery는 Ajax와 관련해서 많은 API를 제공한다.  
그 중 가장 중요한 API는 ```$.ajax()```메소드이다.      
  
**문법**  
```jQuery.ajax( [settings ] )```
```
$.ajax({
        url       :   연결할 파일의 경로를 입력한다.
        type      :   데이터를 전송하는 방법을 지정한다. get, post를 사용할 수 있다.
        data      :   서버로 데이터를 전송할 때 이 옵션을 사용한다. 
        datatype  :   서버측에서 전송한 데이터를 어떤 형식의 데이터로 해석할 것인가를 지정한다.   
                      값으로 올 수 있는 것은 xml, json, script, html이다. 형식을 지정하지 않으면 jQuery가 알아서 판단한다.
        success   :   성공했을 때 호출할 콜백을 지정한다.
                      Function( PlainObject data, String textStatus, jqXHR jqXHR )
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
JQuery는 크로스브라우징 문제를 알아서 해결해주기에   
브라우저에 맞는 추가적인 코드를 작성 안해도 된다.  
또한 개개인 마다 다르지만 코드가 줄고 객체의 프로퍼티 활용을 통해 가독성이 좋아졌다.  

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
    /*★*/  type:'post',
            data:$('form').serialize(),
            success:function(data){
                $('#time').text(data);
            }
        })
    })
</script>
```  
post type은 서버에 데이터를 전송시 URL에 데이터를 붙이지 않고   
BODY에 데이터를 넣어서 보낸다.   
즉 URL 데이터를 붙여서 보내는 GET보다는 보안이 높다.     
   
 ``` data:$('form').serialize(),``` 는 form 태그의 정보를 값의이름=값의내용&값 의 형식으로 바꿔준다.    


***
# 3. JSON 처리 (입력)
## 3.1. time3.php
```
<?php 
$timezones = ["Asia/Seoul", "America/New_York"];
echo json_encode($timezones);
?>
```
## 3.2. demo3.html
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
```dataType:'json'``` 을 통해서 응답받는 데이터 타입을 JSON으로 해석하고   
```success:function(data){ ``` 의 data는 응답 데이터를 의미한다.   
이를 for...in 구문을 이용하여 ```<ul><li>``` 코드를 만들고 이를 id = timezones 에 넣었다.  
