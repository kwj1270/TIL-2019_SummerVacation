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
