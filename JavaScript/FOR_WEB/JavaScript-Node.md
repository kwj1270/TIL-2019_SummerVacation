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
  
그리고 만약 개행 없이 코드가 이루어 져있을 경우 
```
<ul><li><a href="./535">JavaScript Core</a></li>
```
결과는 ``` li ```가 나온다.  
Node 관계 API를 사용한다면 주의하자.  

***
# 2. Node 종류 API
> Node 종류 API는 현재 선택된 노드가 어떤 타입인지 판단할 때 사용한다.
## 2.1. 종류 프로퍼티
```
1.  .nodeType               // node의 type을 의미 (상수로 이루어진 타입들)
2.  .nodeName               // node의 이름을 의미 (태그이름 , text 등의 이름)
```   
nodeType
```
console.dir(Node); 입력하면 나온다.

ATTRIBUTE_NODE: 2
CDATA_SECTION_NODE: 4
COMMENT_NODE: 8
DOCUMENT_FRAGMENT_NODE: 11
DOCUMENT_NODE: 9
DOCUMENT_POSITION_CONTAINED_BY: 16
DOCUMENT_POSITION_CONTAINS: 8
DOCUMENT_POSITION_DISCONNECTED: 1
DOCUMENT_POSITION_FOLLOWING: 4
DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC: 32
DOCUMENT_POSITION_PRECEDING: 2
DOCUMENT_TYPE_NODE: 10
ELEMENT_NODE: 1
ENTITY_NODE: 6
ENTITY_REFERENCE_NODE: 5
NOTATION_NODE: 12
PROCESSING_INSTRUCTION_NODE: 7
TEXT_NODE: 3
```
예시
```
document.body.nodeType      ->      1 (ELEMENT_NODE: 1)
document.body.nodeName      ->      'BODY'      
```

## 2.2. 재귀함수를 이용한 nodeType , nodeName
```
<!DOCTYPE html>
<html>
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
function traverse(target, callback){
/*★*/  if(target.nodeType === 1){
        //if(target.nodeName === 'A')
        callback(target);
/*★*/  var c = target.childNodes;
/*★*/  for(var i=0; i<c.length; i++){
/*★*/       traverse(c[i], callback);       
        }   
    }
}
traverse(document.getElementById('start'), function(elem){
    console.log(elem);
});
</script>
</body>
</html>
```   
**해석**
```
    if(target.nodeType === 1){ 
```
ELEMENT_NODE: 1 인 것들만 실행 시키겠다.
즉 태그요소 이외에 것 들은 무시하겠다.
```
    var c = target.childNodes;   
```
는 모든 자식노드들을 c에 담겠다. (위에서 Text는 걸러진다.즉, 태그들)
```
    for(var i=0; i<c.length; i++){
        traverse(c[i], callback);       
    }
```
자식 노드들의 갯수만큼 반복하는데 자기자신인 traverse메소드를 통해 재귀를 한다  
이유는 자식 노드안에 하위 노드가 존재 할 수 있고 또 그 하위 노드에 하위 노드가 있을 수 있으니  
재귀함수를 넣어서 각 노드마다 자신의 하위 노드를 처리하게끔 한다.  
  
이제 각 태그들만 출력이 될 것이다.  
재귀함수는 넓은 관점에서 보도록하고 추적은 되도록 하지 말자  

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
