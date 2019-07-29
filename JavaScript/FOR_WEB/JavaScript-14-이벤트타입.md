Event Type
=======================

# 1. 폼
## 1.1 submit
> 'submit'은 폼의 정보를 서버로 전송하는 명령인 submit 동작시에 일어난다.

### 1.1.1 예제
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

### 1.2.1 예제
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

### 1.3.1 예제

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

***
# 3. 마우스
## 3.1. 이벤트 타입
```
click               클릭했을 때 발생하는 이벤트. 
dblclick            더블클릭을 했을 때 발생하는 이벤트
mousedown           마우스를 누를 때 발생 (누르고 있는)
mouseup             마우스버튼을 땔 때 발생
mousemove           마우스를 움직일 때
mouseover           마우스가 엘리먼트에 진입할 때 발생
mouseout            마우스가 엘리먼트에서 빠져나갈 때 발생
contextmenu         컨텍스트 메뉴가 실행될 때 발생
```
## 3.2. 키보드 조합
```
event.shiftKey          shit키 누르고 있을때 
event.altKey             alt키 누르고 있을때
event.ctrlKey           crtl키 누르고 있을때

event 프로퍼티이므로 이미 어떠한 이벤트 동작했을 때 같이 조합되는 형태이다.
```
## 3.3. 마우스 포인터 위치
```
clientX             마우스의 x 좌표
clientY             마우스의 y 좌표
```
## 3.4. 예제
```
<html>
    <head>
        <style>
            body{
                background-color: black;
                color:white;
            }
            #target{
                width:200px;
                height:200px;
                background-color: green;
                margin:10px;
            }
            table{
                border-collapse: collapse;
                margin:10px;
                float: left;
                width:200px;
            }
            td, th{
                padding:10px;
                border:1px solid gray;
            }
        </style>
    </head>
    <body>
        <div id="target">
 
        </div>
        <table>
            <tr>
                <th>event type</th>
                <th>info</th>
            </tr>
            <tr>
                <td>click</td>
                <td id="elmclick"></td>
            </tr> 
            <tr>
                <td>dblclick</td>
                <td id="elmdblclick"></td>
            </tr>
            <tr>
                <td>mousedown</td>
                <td id="elmmousedown"></td>
            </tr>         
            <tr>
                <td>mouseup</td>
                <td id="elmmouseup"></td>
            </tr>         
            <tr>
                <td>mousemove</td>
                <td id="elmmousemove"></td>
            </tr>         
            <tr>
                <td>mouseover</td>
                <td id="elmmouseover"></td>
            </tr>         
            <tr>
                <td>mouseout</td>
                <td id="elmmouseout"></td>
            </tr>
            <tr>
                <td>contextmenu</td>
                <td id="elmcontextmenu"></td>
            </tr>         
        </table>
        <table>
            <tr>
                <th>key</th>
                <th>info</th>
            </tr>
            <tr>
                <td>event.altKey</td>
                <td id="elmaltkey"></td>
            </tr>
            <tr>
                <td>event.ctrlKey</td>
                <td id="elmctrlkey"></td>
            </tr>
            <tr>
                <td>event.shiftKey</td>
                <td id="elmshiftKey"></td>
            </tr>
        </table>
        <table>
            <tr>
                <th>position</th>
                <th>info</th>
            </tr>
            <tr>
                <td>event.clientX</td>
                <td id="elemclientx"></td>
            </tr>
            <tr>
                <td>event.clientY</td>
                <td id="elemclienty"></td>
            </tr>
        </table>
        <script>
        var t = document.getElementById('target');
        function handler(event){
            var info = document.getElementById('elm'+event.type);
            var time = new Date();
            var timestr = time.getMilliseconds();
            info.innerHTML = (timestr);
            if(event.altKey){
                document.getElementById('elmaltkey').innerHTML = timestr;
            }
            if(event.ctrlKey){
                document.getElementById('elmctrlkey').innerHTML = timestr;
            }
            if(event.shiftKey){
                document.getElementById('elmshiftKey').innerHTML = timestr;
            }
            document.getElementById('elemclientx').innerHTML = event.clientX;
            document.getElementById('elemclienty').innerHTML = event.clientY;
        }
        t.addEventListener('click', handler);
        t.addEventListener('dblclick', handler);
        t.addEventListener('mousedown', handler);
        t.addEventListener('mouseup', handler);
        t.addEventListener('mousemove', handler);
        t.addEventListener('mouseover', handler);
        t.addEventListener('mouseout', handler);
        t.addEventListener('contextmenu', handler);
        </script>
    </body>
</html>
```

