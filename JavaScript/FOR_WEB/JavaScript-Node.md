Node 객체
=======================
> Node 객체는 DOM에서 시조와 같은 역할을 한다.  
> 다시 말해서 모든 DOM 객체는 Node 객체를 상속 받는다.  

![Node](https://user-images.githubusercontent.com/50267433/62005180-4db70300-b16a-11e9-8450-4ac7ebb0460e.png)

# 1. Node관계 API
> Node 객체는 Node 간의 관계 정보를 담고있는 일련의 API를 가지고 있다.
## 1.1 관계 프로퍼티
```
노드는 Node객체에 속한 요소를 뜻하며 
자주 사용하는 요소 객체들도 있지만 
text이런것도 node로 취급한다 . 즉 개행도 노드로 취급한다.

1.  .chidNodes              // 모든 자식 노드들
2.  .firstChild             // 첫번째 자식 노드만
3.  .lastChild              // 마지막 자식 노드만
4.  .nextChild              // 다음 형제 노드
5.  .previousChild          // 이전 형제 노드
6.  .parentNode             // 부모 노드
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
Node는 Node 객체내에 속하는 객체를 말한다 했다.
```console.log(3, ul.nextSibling);```를 보면           
```
<ul>
            <li><a href="./535">JavaScript Core</a></li>
```
이 코드를 가리키는 것인데 ```<ul>``` 과 ```<li>```사이 빈공간에 개행이 있기에 ```#text```가 나오는 것이다.


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
