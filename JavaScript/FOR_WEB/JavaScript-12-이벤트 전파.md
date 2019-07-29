이벤트-전파 (버블링 , 캡처링)
=======================
> HTML은 Tree구조로 부모 자식 관계를 가진다.  
> 이는 이벤트가 발생시 중첩된 태그들도 대상이 될 수 있다.  
  
![버블링과 캡처링 구조](https://user-images.githubusercontent.com/50267433/62018189-9b784d80-b1f4-11e9-9763-b9be95ce3670.PNG)
  
**HTML 코드**
```
<html>
    <head>
        <style>
            html{border:5px solid red;padding:30px;}
            body{border:5px solid green;padding:30px;}
            fieldset{border:5px solid blue;padding:30px;}
            input{border:5px solid black;padding:30px;}
        </style>
    </head>
    <body>
        <fieldset>
            <legend>event propagation</legend>
            <input type="button" id="target" value="target">          
        </fieldset>
    </body>
</html>
```

# 1. 캡처링
> 부모요소부터 자식 요소 순으로 이벤트가 발생하는 것

## 1.1. 코드
 ```
 <script>
        function handler(event){
            var phases = ['capturing', 'target', 'bubbling'] //0 , 1 , 2
            console.log(event.target.nodeName, this.nodeName, phases[event.eventPhase-1]); // 1 , 2 , 3 이므로 -1
        }
        document.getElementById('target').addEventListener('click', handler, true);
        document.querySelector('fieldset').addEventListener('click', handler, true);
        document.querySelector('body').addEventListener('click', handler, true);
        document.querySelector('html').addEventListener('click', handler, true);
 </script>
 ```
이벤트가 발생하면      
```
event.target.nodeName : 실제 이벤트가 일어난 node의 name.  
this.nodeName : 현재 이벤트가 실행 되는 node의 name 
                (버블링, 캡처링시에는 마지막까지 가니까 마지막 node의 name)  
phases[event.eventPhase-1] : 이벤트 전파 종류 출력 'capturing', 'target', 'bubbling'  
```
```event.eventPhase-1```을 사용한 이유는  
```event.eventPhase``` 는 아래와 같이 구성 되어있다.
```
Event.CAPTURING_PHASE(캡쳐링)	1
Event.AT_TARGET(타겟)	2
Event.BUBBLING_PHASE(버블링)	3
```
이는 인덱스 값 0-2 까지 1 만큼 차이가 나므로 -1을 사용한 것이다.
## 1.2. 결과
```
INPUT HTML capturing
INPUT BODY capturing
INPUT FIELDSET capturing
INPUT INPUT target
```
***
# 2. 버블링
> 자식요소부터 부모요소 순으로 이벤트가 발생하는 것  
## 2.1. 코드
```
document.getElementById('target').addEventListener('click', handler, false);
document.querySelector('fieldset').addEventListener('click', handler, false);
document.querySelector('body').addEventListener('click', handler, false);
document.querySelector('html').addEventListener('click', handler, false);

addEventListener의 3번째 인자가 true 에서 false로 바뀌었다.
3번째 인자는 생략하면 fasle 가 된다.
```
### 2.2. 결과
```
INPUT INPUT target
INPUT FIELDSET bubbling
INPUT BODY bubbling
INPUT HTML bubbling
```

***
# 3. 이벤트 중단 stopPropagation()
## 3.1. 코드
```
function handler(event){
    var phases = ['capturing', 'target', 'bubbling']
    console.log(event.target.nodeName, this.nodeName, phases[event.eventPhase-1]);
}
function stophandler(event){
    var phases = ['capturing', 'target', 'bubbling']
    console.log(event.target.nodeName, this.nodeName, phases[event.eventPhase-1]);
    event.stopPropagation();
}
document.getElementById('target').addEventListener('click', handler, false);
document.querySelector('fieldset').addEventListener('click', handler, false);
document.querySelector('body').addEventListener('click', stophandler, false);
document.querySelector('html').addEventListener('click', handler, false);

```
## 3.2. 결과
```
INPUT INPUT target
INPUT FIELDSET bubbling
INPUT BODY bubbling
```
event.stopPropagation() : 버블링, 캡쳐링 중단.

