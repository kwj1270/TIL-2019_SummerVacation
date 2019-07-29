이벤트-전파 (버블링 , 캡처링)
=======================
> HTML은 Tree구조로 부모 자식 관계를 가진다.  
> 이는 이벤트가 발생시 중첩된 태그들도 대상이 될 수 있다.  
  
![버블링과 캡처링 구조](https://user-images.githubusercontent.com/50267433/62018189-9b784d80-b1f4-11e9-9763-b9be95ce3670.PNG)

1. 캡처링 : 부모요소부터 자식요소 순으로 이벤트가 발생하는 것
2. 버블링 : 자식요소부터 부모요소 순으로 이벤트가 발생하는 것  
** HTML 코드 **
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
    </body>
</html>
```

# 1. 캡처링
> 부모요소부터 자식 요소 순으로 이벤트가 발생하는 것

## 1.1 소 주제
### 1.1.1. 내용1
```
내용1
```
## 1.2. 소 주제
### 1.1.1. 내용1
```
내용1
```

***
# 2. 대주제
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
