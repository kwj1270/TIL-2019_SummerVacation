BOM(Browser Object Model)
=======================
> 웹 브라우저의 창 또는 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단

# 1. Window
> window객체는 모든 객체의 상위 객체이다
## 1.1 전역객체 Window
### 1.1.1. 메소드
```
window.alert();
alert();
같은 표현 
window는 생략 가능하다.
```
### 1.1.2. 변수
```
var a = 1;
alert(window.a);
alert(a);
같은 표현
```
### 1.1.3. 객체
```
var b = { id:1 };
alert(window.b.id);
alert(b.id);
```

## 1.2. 창/프레임 window
### 1.2.1. open()메소드
```
window.open("URL");
window.open("URL", "target 값");       // '_blank','_self','_parent','_top' 
window.open("URL", "ot");              // 없으면 새창 , 있으면 reload 
window.open("URL", "target 값" , "브라우저 창 속성 값");           
```
### 1.2.2. close()메소드
```
let win = window.open('demo.html' , 'ot' , 'width = 300px , height = 500px');
win.close(); 
```

## 1.3 새로운 window와 커뮤니케이션
새창에 대한 객체 win을 통하여  
onkeypress이벤트 발생시 새창에 text가 입력된다.
### 1.3.1. demo1.html
```
<!DOCTYPE html>
<html>
<body>
    <input type="button" value="open" onclick="winopen();" />
    <input type="text" onkeypress="winmessage(this.value)" />
    <input type="button" value="close" onclick="winclose()" />
    <script>
    function winopen(){
        win = window.open('demo2.html', 'ot', 'width=300px, height=500px');
    }
    function winmessage(msg){
        win.document.getElementById('message').innerText=msg;
    }
    function winclose(){
        win.close();
    }
    </script>
</body>
</html>
```
### 1.3.2. demo2.html
```
<!DOCTYPE html>
<html>
<body>
<p id = "message"></p>
</body>
</html>
```














***
# 2. 사용자와 커뮤니케이션
> 인용
## 2.1. 
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
