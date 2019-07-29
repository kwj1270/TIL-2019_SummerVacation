HTML에서 JavaScript 로드하기
=======================
# 1. INLINE
> HTML태그에 직접 JavaScript를 기술하는 방식이다.
## 1.1. 장 단점
### 1.1.1. 장점
```
태그에 연관된 스크립트가 분명하게 드러난다
```
### 1.1.2. 단점
```
정보와 제어가 섞여있기에 정보로서 가치가 떨어진다.(유지보수 및 검색엔진기능 저하)
```      
## 1.2. 예제
```
<input type="button" onclick="alert('Hello world')" value="Hello world" />
```   
간략히 해석하자면 버튼 클릭시 Hello world 라는 경고창을 띄운다.  

***
# 2. SCRIPT
> <script>태그 안에 JavaScript를 기술하는 방식  
## 2.1. 장점
```
HTML 태그와 JS코드를 분리하여 INLINE의 단점을 어느정도 해소화
```
## 2.2. 예제
```
<body>
    <input type="button" id="hw" value="Hello world" />
    <script type="text/javascript">
        var hw = document.getElementById('hw');
        hw.addEventListener('click', function(){
            alert('Hello world');
        })
    </script>
</body>
```   

***
# 3. 외부파일 호출
> HTML과 JS를 별도의 파일로 분리할 수 있다.  
## 3.1. 장점
```
1. 보다 엄격히 정보(HTML)와 제어(JS)를 분리
2. 하나의 JS파일을 사용하여 여러 HTML에 적용 가능
3. Cache를 통한 속도의 향상, 전송량의 경량화를 도모

즉 유지보수의 편의성 제공 및 작업시간 단축
``` 
## 3.2. 예제
### 3.2.1. HTML 코드
```
<!DOCTYPE html>
<html>
<body>
    <input type="button" id="hw" value="Hello world" />
    <script type="text/javascript" src="script2.js"></script>
</body>
</html>
```
### 3.2.2. JavaScript 코드
```
var hw = document.getElementById('hw');
hw.addEventListener('click', function(){
    alert('Hello world');
})
```
