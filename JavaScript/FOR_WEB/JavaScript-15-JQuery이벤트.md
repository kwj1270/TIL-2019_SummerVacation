JQuery 이벤트 
=======================
> JQuery는 이벤트와 관련해서 편리한 기능을 제공한다.

# 1. 코드 비교
## 1.1 순수 js 코드
```
<input type="button" id="pure" value="pure" />
<input type="button" id="jquery" value="jQuery" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    var target = document.getElementById('pure');
    if(target.addEventListener){
        target.addEventListener('click', function(event){
            alert('pure');
        });
    } else {
        target.attachEvent('onclick', function(event){
            alert('pure');
        });
    }
</script>
```
순수 JavaScript 코드는 addEventListener 같이  
호환이 안 되는 브라우저에도 맞춰서 코드를 작성 해주어야 한다.  
## 1.2 JQuery
```
<input type="button" id="pure" value="pure" />
<input type="button" id="jquery" value="jQuery" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
   $('#jquery').on('click', function(event){
        alert('jQuery');
    })
</script>
```
JQuery는 CrossBrowsing 에 상관없이 제이쿼리 코드로 작성하면 알아서 맞추어 준다.
이러한 이유로 많은 사람들이 라이브러리를 이용한다.
단, 라이브러리가 꼭 옳다 는 관점은 아니다. 프로그래머라면 편식은 옳지 않다.
# 2. on API
> 인용
## 2.1. 소 주제
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
