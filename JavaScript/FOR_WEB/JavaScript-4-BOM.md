BOM(Browser Object Model)
=======================
```
BOM : 웹 브라우저의 창 또는 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
```
# 1. Window
> window객체는 모든 객체의 상위 객체이다
## 1.1. 전역객체 Window
### 1.1.1. 메소드
```
window.alert();
alert();
```
window는 생략 가능하다.
### 1.1.2. 변수
```
var a = 1;
alert(window.a);
alert(a);
```
window는 생략 가능하다.
### 1.1.3. 객체
```
var b = { id:1 };
alert(window.b.id);
alert(b.id);
```
window는 생략 가능하다.
## 1.2. 창/프레임 window
### 1.2.1. open()메소드
```
window.open("URL");
window.open("URL", "target 값");       // '_blank','_self','_parent','_top' 
window.open("URL", "ot");              // 없으면 새창 , 있으면 reload 
window.open("URL", "target 값" , "브라우저 창 속성 값");           
```
target 값은 HTML의 target 속성의 값을 의미한다.  
```
* ```'_blank'```  : 새 프레임에서 열기  
* ```'_self'```   : 현재 프레임에서 열기  
* ```'_parent'``` : 부모 프레임에서 열기  
* ```'_top'```    : 최상위 프레임에서 열기 즉 가장 선조프레임 에서 연다  
```
브라우저 창 속성 값은 가로 , 세로 등을 나타낸다  
### 1.2.2. close()메소드
```
let win = window.open('demo.html' , 'ot' , 'width = 300px , height = 500px');
win.close(); 
```

## 1.3. 새로운 window와 커뮤니케이션
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
### 1.3.3. 실행 결과
![newWindowComunication](https://user-images.githubusercontent.com/50267433/62002459-770e6980-b13f-11e9-867e-5930128cbcae.gif)  

## 1.4 팝업차단
### 1.4.1. demo3.html
```
<!DOCTYPE html>
<html>
<body>
    <script>
    window.open('demo2.html');
    </script>
</body>
</html>
```

사용자의 인터렉션 없이 창을 열려고 하면 팝업이 차단 된다.  
즉, 사용자의 고의적 접근이 아니면 팝업을 띄워서 접속 여부를 묻고   
이는 사용자에게 문제발생시에 대한 책임을 이전시킨다.  
  
브라우저 설치시 브라우저는 사용자의 컴퓨터에 접근을 할 수 있는 권한을 가진다.  
하지만 만약 어느 특정 페이지에서 브라우저를 조작하여 사용자의 데이터를 가져갈 수 있기에  
같은 도메인을 사용하는 사이트끼리만 JS로 제어 할 수 있다.  
팝업은 이러한 보안 책임을 사용자에게 이전하는 것 이다.  


***
# 2. 사용자와 커뮤니케이션
> 사용자와의 커뮤니케이션으로 자바스크립트의 제어동작이 달라진다.
## 2.1. alert
```
alert('내용');
alert("현재 서버 점검중입니다.")
```   
경고창이 실행되는 동안 다른 코드들은 실행되지 않는다.
## 2.2. confirm
```
confirm("질문");
confirm("ok?");
확인 = true / 취소 = false
```   
사용자에게 물음을 던져 사용자의 선택에 따라 값이 true/false로 반환된다.
## 2.3. prompt
```
prompt("질문","초기값");
prompt("id?");
prompt("id?", "");
prompt("number","0");
```  
사용자로부터 값을 입력 받을수 있다.

***
# 3. Location 객체
> 문서의 주소와 관련된 객체로   
> window(창)의 문서 URL 변경 및 정보를 얻을 수 있다.
## 3.1. 현재 URL
```
console.log(location.toString());
console.log(location.href);
alert(location);                    // alert은 문자열만 사용 가능하기에 location 객체를 문자열로 자동 치환
```
## 3.2. URL Parsing
```
console.log(    lacation.protocol       // http
                        .host           // 도메인
                        .port           // port 번호
                        .pathname       // 경로
                        .search         // ? 파라미터 (queryString)
                        .hash           // # 앵커

);

http://opentutorials.org :80 /module/1?id=1#hash ->생활코딩 CSS 강의
프로토콜        도메인    포트    경로 파라미터 앵커  

```
## 3.3. URL 변경
```
location.href = "새로운 경로";      // 권장
location = "새로운 경로";           
```

## 3.4. 웹 페이지 reload
```
location.reload;                    // 권장
location.href = location.href;      
```
# 4. Navigator 객체
> 브라우저 정보를 제공하는 객체, 주로 호환성 문제등을 위해서 사용된다.  
> 현재 실행되는 브라우저의 제품명, 버전등을 알 수 있는 객체

## 4.1. CrossBrowsing
```
W3C, ECMA 국제 표준에 따라 각 브라우저 공급 업체들은 브라우저를 만들지만
각 브라우저마다 세밀한 부분까지는 다르다.
그래서 같은 코드를 사용하더라도 다른 결과를 초래할수 있고 이를 CrossBrowsing issue 라 한다.

고로 우리는 브라우저의 특성에 맞춰 코드를 작성해야 한다.
```
## 4.2. Navigator 객체와 프로퍼티
### 4.2.1. navigator 객체 조회
```
console.dir(navigator);
```
Navigator 객체의 프로퍼티 열람 (list 형식)
### 4.2.2. .appName
```
console.dir(navigator.appName);
```
웹 브라우저의 이름 출력 (일부 브라우저는 Netscape로 정의) 
### 4.2.3. .appVersion
```
console.dir(navigator.appVersion);
```
웹 브라우저 정보 출력 (이름, 버전 등)
### 4.2.4. .userAgent
```
console.dir(navigator.userAgent);
```
웹 브라우저가 웹 서버에 요청시 보내는 웹 브라우저에 대한 정보(헤더)  
USER-AGENT HTTP 헤더의 내용
### 4.2.5. .platform
```
console.dir(navigator.platform);
```
운영 체제 정보
