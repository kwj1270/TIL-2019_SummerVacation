문서의 기하학적 특성
=======================
> 문서에 존재하는 엘리먼트들의 크기와 위치를 제어하는 것 

# 1. 요소의 크기와 위치
## 1.1 메소드
```
getBoundingClientRect()         // 요소의 크기와 위치를 나타내는 메소드
                                // ViewPort기준으로 값을 보여준다.
```
### 1.1.1. 예제
```
<style>
    body{
        padding:0;
        margin:0;
    }
    #target{
        width:100px;
        height:100px;
        border:50px solid #1065e6;
        padding:50px;
        margin:50px;
    }
</style>
<div id="target">
    Coding
</div>
<script>
var t = document.getElementById('target');
console.log(t.getBoundingClientRect());
</script>
```
**엘리먼트 중첩시**
```
<style>
    body{
        padding:0;
        margin:0;
    }
    div{
        border:50px solid #1065e6;
        padding:50px;
        margin:50px;
    }
    #target{
        width:100px;
        height:100px;
    }
</style>
<div>
    <div id="target">
        Coding
    </div>
</div>
<script>
var t = document.getElementById('target');
console.log(t.getBoundingClientRect());
console.log(t.offsetParent);
</script>
```
**테두리 제외한 엘리먼트 크기**
```
<script>
var t = document.getElementById('target');
console.log('clientWidth:', t.clientWidth, 'clientHeight:', t.clientHeight);
</script>
```

***
# 2. Viewport
> 브라우저에서 사용자에게 보여주는 영역(창)을 의미한다.
## 2.1. 예제
```
<style>
    body{
        padding:0;
        margin:0;
    }
    div{
        border:50px solid #1065e6;
        padding:50px;
        margin:50px;
    }
    #target{
        width:100px;
        height:2000px;
    }
</style>
    <div>
        <div id="target">
            Coding
        </div>
    </div>
 
<script>
var t = document.getElementById('target');
setInterval(function(){
    console.log('getBoundingClientRect : ', t.getBoundingClientRect().top, 'pageYOffset:', window.pageYOffset);
}, 1000)
</script>
```   
### 2.1.1 문서 기준 좌표
기존 코드의 일부분만 수정한다. 
```
setInterval(function(){
    console.log('getBoundingClientRect : ', t.getBoundingClientRect().top, 'pageYOffset:', window.pageYOffset, 'document y:', t.getBoundingClientRect().top+window.pageYOffset);
}, 1000)
```
### 2.1.2. 프로퍼티 및 메소드 정리
```
getBoundingClientRect();                        // 뷰 포트 기준 좌표 출력
setInterval(function , time_seconds);           // function을 x초(1초에 1회) 만큼 실행

pageYOffset;                                    // 문서 기준 스크롤된 좌표 출력 (세로)
pageXOffset;                                    // 문서 기준 스크롤된 좌표 출력 (가로)
```
문서 기준 좌표 값은 뷰포트의 좌표에 스크롤된 정도를 더해서 알 수 있다. 
***
# 3. 스크롤
## 3.1. 메소드 및 프로퍼티
```
scrollTo(x,y);      //  x,y 만큼 스크롤을 이동해라
scrollLeft;         //  x 값 -> 수정해서 사용가능      
scrollTop;          //  y 값 -> 수정해서 사용가능

```
### 3.1.1 예제
```
<style>
    body{
        padding:0;
        margin:0;
    }
    div{
        border:50px solid #1065e6;
        padding:50px;
        margin:50px;
    }
    #target{
        width:100px;
        height:2000px;
    }
</style>
<input type="button" id="scrollBtn" value="scroll(0, 1000)" />
<script>
    document.getElementById('scrollBtn').addEventListener('click', function(){
        window.scrollTo(0, 1000);
    })
</script>
<div>
    <div id="target">
        Coding
    </div>
</div>
```

***
# 4. 스크린의 크기
스크린의 크기는  
1. 디바이스 모니터의 크기 
2. 브라우저 뷰포트 크기
## 4.1.프로퍼티
```
window.innerWidth         // 브라우저 뷰포트 가로 길이
window.innerHeight        // 브라우저 뷰포트 세로 길이

screen.innerWidth         // 디바이스 모니터 가로 길이
screen.innerHeight        // 디바이스 모니터 세로 길이

```
### 4.1.1. 예제
```
<script>
console.log('window.innerWidth:', window.innerWidth, 'window.innerHeight:', window.innerHeight);
console.log('screen.width:', screen.width, 'screen.height:', screen.height);
</script>
```
