문서의 기하학적 특성
=======================
> 문서에 존재하는 엘리먼트들의 크기와 위치를 제어하는 것 

# 1. 요소의 크기와 위치
> 인용
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
## 1.2. 소 주제
### 1.1.1. 내용1
```
내용1
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
문서 기준 좌표 값은 뷰포트의 좌표에 스크롤된 정도를 더해서 알 수 있다. 
***
# 3. 대주제
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```
