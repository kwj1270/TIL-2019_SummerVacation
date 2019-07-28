Node 객체
=======================
> Node 객체는 DOM에서 시조와 같은 역할을 한다.  
> 다시 말해서 모든 DOM 객체는 Node 객체를 상속 받는다.  
![Node](https://user-images.githubusercontent.com/50267433/62005180-4db70300-b16a-11e9-8450-4ac7ebb0460e.png)

# 1. Node관계 API
> Node 객체는 Node 간의 관계 정보를 담고있는 일련의 API를 가지고 있다.
## 1.1 관계 프로퍼티
```
1.  .chidNodes
2.  .firstChild
3.  .lastChild
4.  .nextChild
5.  .previousChild
6.  .parentNode
```
**예시**  
```
<body id="start">
<ul>
    <li><a href="./532">html</a></li> 
    <li><a href="./533">css</a></li>
    <li><a href="./534">JavaScript</a>
        <ul>
            <li><a href="./535">JavaScript Core</a></li>
            <li><a href="./536">DOM</a></li>
            <li><a href="./537">BOM</a></li>
        </ul>
    </li>
</ul>
<script>
var s = document.getElementById('start');
console.log(1, s.firstChild);               // #text
var ul = s.firstChild.nextSibling
console.log(2, ul); // ul
console.log(3, ul.nextSibling);             // #text
console.log(4, ul.nextSibling.nextSibling); // script
console.log(5, ul.childNodes);              //text, li, text, li, text, li, text
console.log(6, ul.childNodes[1]);           // li(html)
console.log(7, ul.parentNode);              // body
</script>
</body>
```

***
# 2. Node 종류 API
> 인용
## 2.1. 프로퍼티
```
내용1
```   
## 2.2. 재귀함수를 이용한 nodeType , nodeName
```
내용1
```   


***
# 3. Node 변경 API
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```

***
# 4. JQuery 노드변경 API
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```
