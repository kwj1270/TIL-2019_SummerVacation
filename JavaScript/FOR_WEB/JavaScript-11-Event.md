EVENT
=======================
> 어떠한 동작이나 사건을 의미  

**자바스크립트 이벤트 구성요소**  

```
1. event target           // 이벤트 대상을 의미  
2. event type             // 이벤트 종류
3. event handler          // 이벤트 동작(행동)
```

# 1. 이벤트 등록 방법
## 1.1 INLINE
> 이벤트 대상의 태그속성을 이용하여 이벤트 등록 
### 1.1.1. 기본 방식
```
<input type = "button" oncilck = "alert('hello world');" value = "button"/>
               대상       종류         동작
```
이벤트 대상 : button  
이벤트 종류 : onclick  
이벤트 동작 : alert('hello world')이다.  
  
즉 button이 click 되면  alert('hello world')을 발생시켜라 라는 의미가 된다.
### 1.1.2. 자기 자신 이벤트 등록
**불편한 방법** 
```
<input type="button" id="target" onclick="alert('Hello world, '+document.getElementById('target').value);" value="button" />
```
**this를 활용한 방법**
```
<input type="button" onclick="alert('Hello world, '+this.value);" value="button" />
```
this 객체는 원래는 자기 자신을 호출한 대상을 의미하는데(일반적으로 window 객체안에서는 객체 의미)  
이벤트에서는 이 this를 event target 즉 이벤트 대상으로 변경시켰다.   
그래서 이벤트 대상을 의미하는 this 객체를 이용하면 조금더 간편하게 코드를 작성 할 수 있다.  

## 1.2. 프로퍼티 리스너
> 이벤트 대상에 해당하는 객체의 프로퍼티로 이벤트를 등록하는 방식  
> <script> 태그에 이벤트를 등록하는 방식이다.
### 1.2.1. 기본방식
  
```
<script>
    let t = document.getElementById('target');
    t.onclick = function(event){
          alert('Hello world');
    }       
</script>
```
이벤트 대상 : t (document.getElementById('target'))
이벤트 종류 : onclick  
이벤트 동작 : function(event){alert('Hello world')}이다.  

이벤트 헨들러(동작)인 function(event){alert('Hello world')}은  
Event 객체 자체를 받을 수 있고 이를 활용 할 수 있다.  
```
console.log(event.target.value);
```
Event 객체는 여러 프로퍼티가 있는데  
그중 target이라는 프로퍼티를 이용하면 현재 이벤트 동작이 일어나는 대상을 의미한다.  
정확히는 event.target은 이벤트 버블링의 가장 마지막에 위치한 최하위 요소를 반환한다.
```
<div id = "target">
  <span>Attention</span>
</div>

<script>
let t = document.getElementById('target');
t.onclick = function(event){
    event.target.value
    event.currentTarget.value
}
</script>
```
event.target.value  : span 태그를 반환  
event.currentTarget.value  : div 를 반환  
이런 점에서 사용할 때 주의를 해야한다.  
### 1.2.2. IE8 이하  
**IE8 이하**
```
<input type="button" id="target" value="button" />
<script>
    var t = document.getElementById('target');
    t.onclick = function(event){
        var event = event || window.event;
        var target = event.target || event.srcElement;
        alert('Hello world, '+target.value)
    }
</script>
```
IE8이하 버전에서는 event 객체를 받지못하니 위와 같이 사용한다.  
event 객체가 있으면 매개변수로 받은 event를 사용하고  
없으면 직접 window.event로 값을 넣어주자

## 1.3. addEventListner
> 여러개의 event handler를 등록할 수 있다. 
### 1.3.1. 기본방식
```
<input type="button" id="target" value="button" />
<script>
    var t = document.getElementById('target');
    t.addEventListener('click', function(event){
        alert('Hello world, '+event.target.value);
    });
</script>
```
이벤트 대상 : t (document.getElementById('target'))
이벤트 종류 : 'click'  
이벤트 동작 : function(event){alert('Hello world, '+event.target.value);}이다.   
  
프로퍼티 리스너와 마찬가지로 Event 객체 자체를 받을 수 있고 이를 활용 할 수 있다.  
기존 방식들과 다른점은 1개의 대상에 여러 이벤트를 넣을 수 있다.  
**예제**
```
<input type="button" id="target" value="button" />
<script>
    var t = document.getElementById('target');
    t.addEventListener('click', function(event){
        alert(1);
    });
    t.addEventListener('click', function(event){
        alert(2);
    });
</script>
```
기존 방식들은 같은 event type에 대해서 중복을 허용하지 않았지만  
addEventListener 방식은 같은 event type을 사용해도 중복을 허용하여 같이 동작한다.  
### 1.3.2. IE8 이하
**IE8 이하**
```
var t = document.getElementById('target');
if(t.addEventListener){
    t.addEventListener('click', function(event){
        alert('Hello world, '+event.target.value);
    }); 
} else if(t.attachEvent){
    t.attachEvent('onclick', function(event){
        alert('Hello world, '+event.target.value);
    })
}
```
IE8이하 버전에서는 addEventListener 가 호환되지 않으니   
attachEvent메소드를 사용해야 한다.

### 2.1. 부록
addEventListener 는 function을 따로 정의해서 사용 할 수 있다.
```
<input type="button" id="target1" value="button1" />
<input type="button" id="target2" value="button2" />
<script>
    var t1 = document.getElementById('target1');
    var t2 = document.getElementById('target2');
    function btn_listener(event){
        switch(event.target.id){
            case 'target1':
                alert(1);
                break;
            case 'target2':
                alert(2);
                break;
        }
    }
    t1.addEventListener('click', btn_listener);
    t2.addEventListener('click', btn_listener);
</script>
```
위 예제를 보면 이벤트 객체를 이용하여 복수의 엘리먼트에 하나의 리스너를 등록해서 재사용하고 있다. 
프로퍼티 리스너 방식은 ``` 대상.타입 = 핸들러(동작) ``` 이기에 사용 할 수가 없다.
이렇듯 addEventListener는 다른 방식에 비해 사용하기가 편하다.
