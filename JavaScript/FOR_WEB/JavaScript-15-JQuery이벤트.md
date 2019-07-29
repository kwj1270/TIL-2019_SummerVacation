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
JQuery는 선택자와 이벤트를 단순한 코드로 작성 할 수 있다.  
또한 CrossBrowsing 에 상관없이 제이쿼리 코드로 작성하면 알아서 맞추어 준다.  
이러한 이유로 많은 사람들이 라이브러리를 이용한다.  
단, 라이브러리가 꼭 옳다 는 관점은 아니다. 프로그래머라면 편식은 옳지 않다.  

***
# 2. ON API
> on은 jQuery에서 가장 중요한 이벤트 API이다.
## 2.1. 기본 사용법
```
.on( events [, selector ] [, data ], handler(eventObject) )

event : 등록하고자 하는 이벤트 타입을 지정한다. (예: "click")
selector : 이벤트가 설치된 엘리먼트의 하위 엘리먼트를 이벤트 대상으로 필터링함
data : 이벤트가 실행될 때 핸들러로 전달될 데이터를 설정함
handler : 이벤트 핸들러 함수

selector 와 data는 생략이 가능하다
다만 존재 유무에 따라 동작을 다르게 실행한다.
```   
## 2.2. [, selector]
> selector 파라미터는 이벤트 대상을 필터링 한다.
```
<ul>
    <li><a href="#">HTML</a></li>
    <li><a href="#">CSS</a></li>
    <li><a href="#">JavaScript</a></li>
</ul>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('ul').on('click','a, li', function(event){
        console.log(this.tagName);
    })
</script>
```
## 2.3. late binding
> jQuery는 존재하지 않는 엘리먼트에도 이벤트를 등록할 수 있는 놀라운 기능을 제공한다.
```
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('body').on('click','a, li', function(event){
        console.log(this.tagName);
    })
</script>
<ul>
    <li><a href="#">HTML</a></li>
    <li><a href="#">CSS</a></li>
    <li><a href="#">JavaScript</a></li>
</ul>
```
late binding은 선택자가 존재하면 [, selector]가 나중에 정의되어도 이벤트를 적용할 수 있다.  
단 몇가지 주의점이 있다.  
1. 선택자가 먼저 정의되어 있지 않으면 안된다.  
2. [, selector]가 나중에라도 정의되어 있지 않다면 안된다.  
## 2.4. 다중 바인딩
```
<input type="text" id="target" />
<p id="status"></p>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#target').on('focus blur', function(e){
        $('#status').html(e.type);
    })
</script>
```

```
<input type="text" id="target" />
<p id="status"></p>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#target').on({
        'focus' : function(e){
 
        }, 
        'blur' : function(e){
             
        }
    })
</script>
```
## 2.5. 이벤트 제거
```
<input type="text" id="target"></textarea>
<input id="remove"  type="button" value="remove" />
<p id="status"></p>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
  var handler = function(e){
    $('#status').text(e.type+Math.random());
  };
  $('#target').on('focus blur', handler)
  $('#remove').on('click' , function(e){
    $('#target').off('focus blur', handler);
    console.log(32);
  })
</script>
```
