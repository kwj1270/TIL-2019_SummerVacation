Node 객체
=======================
> Node 객체는 DOM에서 시조와 같은 역할을 한다.  
> 다시 말해서 모든 DOM 객체는 Node 객체를 상속받는다.  

![Node](https://user-images.githubusercontent.com/50267433/62005180-4db70300-b16a-11e9-8450-4ac7ebb0460e.png)

# 1. Node 관계 API
> Node 객체는 Node 간의 관계 정보를 담고 있는 일련의 API를 가지고 있다.  
## 1.1. 관계 프로퍼티
```
노드는 Node 객체에 속한 요소를 뜻하며 
자주 사용하는 요소 객체들도 있지만 
text 이런 것도 node로 취급한다. 즉 개행도 노드로 취급한다.

1.  .chidNodes              // 모든 자식 노드들
2.  .firstChild             // 첫 번째 자식 노드만
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
console.log(5, ul.childNodes);              // text, li, text, li, text, li, text
console.log(6, ul.childNodes[1]);           // li(html)
console.log(7, ul.parentNode);              // body
</script>
</body>
```
Node는 Node 객체 내에 속하는 객체를 말한다.  
```console.log(3, ul.nextSibling);```를 보면           
```
<ul>
            <li><a href="./535">JavaScript Core</a></li>
```
이 코드를 가리키는 것인데 ```<ul>``` 과 ```<li>```사이 빈 공간에 개행이 있기에 ```#text```가 나오는 것이다.  
  
그리고 만약 개행 없이 코드가 이루어져 있을 경우 
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
ELEMENT_NODE: 1 인 것들만 실행시키겠다.  
즉 태그 요소 이외에 것 들은 무시하겠다.  
```
    var c = target.childNodes;   
```
는 모든 자식 노드들을 c에 담겠다. (태그들만 존재)  
```
    for(var i=0; i<c.length; i++){
        traverse(c[i], callback);       
    }
```
자식 노드들의 개수만큼 반복하는데 자기 자신인 traverse 메소드를 통해 재귀를 한다  
이유는 자식 노드 안에 하위 노드가 존재할 수 있고 또 그 하위 노드에 하위 노드가 있을 수 있으니  
재귀 함수를 넣어서 각 노드마다 자신의 하위 노드를 처리하게끔 한다.  
  
이제 각 태그들만 출력이 될 것이다.  
재귀 함수는 넓은 관점에서 보도록 하고 추적은 되도록 하지 말자  

***
# 3. Node 변경 API
> 노드를 추가, 제거, 변경 할 수 있다.  
## 3.1. 추가
### 3.1.1. 엘리멘트 생성 메소드
```
1.  document.createElement(tagname);        // 엘리멘트 생성(태그)
2.  document.createTextNode(data);          // text 생성
```
### 3.1.2. 추가 메소드
```
1. .appendChild(node)                               // 노드의 마지막 자식으로 주어진 엘리먼트 추가
2. insertBefore(new Element , referenceElement)     // 두번째 인자 엘리먼트 앞쪽에 엘리먼트 추가
```
### 3.1.3. 예제
```
<ul id="target">
    <li>HTML</li>
    <li>CSS</li>
</ul>
<input type="button" onclick="callAppendChild();" value="appendChild()" />
<input type="button" onclick="callInsertBefore();" value="insertBefore()" />
<script>
    function callAppendChild(){
        var target = document.getElementById('target');
        var li = document.createElement('li');
        var text = document.createTextNode('JavaScript');
        li.appendChild(text);
        target.appendChild(li);
    }
    // <li>JavaScript</li>를 <ul>의 마지막 자식으로 놓겠다. 
    
    function callInsertBefore(){
        var target = document.getElementById('target');
        var li = document.createElement('li');
        var text = document.createTextNode('jQuery');
        li.appendChild(text);
        target.insertBefore(li, target.firstChild);
    }
    
    // <li>Jquey</li>를 <ul>의 첫번째 자식 앞으로 놓겠다. 즉 첫번째로 놓겠다.
</script>
```
1. ```<li>JavaScript</li>```를 ```<ul>```의 마지막 자식으로 놓겠다.   
2. ```<li>Jquey</li>```를 ```<ul>```의 첫번째 자식 앞으로 놓겠다. 즉 첫번째로 놓겠다.  

## 3.2. 삭제 
### 3.2.1. 삭제 메소드
```
.removeChild(childNode);        // 특정 자식 노드를 삭제한다.
```
### 3.2.2. 예제
```
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="target">JavaScript</li>
</ul>
<input type="button" onclick="callRemoveChild();" value="removeChild()" />
<script>
    function callRemoveChild(){
        var target = document.getElementById('target');
        target.parentNode.removeChild(target);
    }
</script>
```
target이 자기 자신을 삭제하기 위해 부모노드로 이동 후 자기 자신을 삭제하는 메소드 호출  
## 3.3. 변경 
### 3.3.1. 변경 메소드
```
.reaplaceChild(newChild , oldChild)     // 2번째 자식노드를 1번째 자식노드로 변경한다.
```
### 3.3.2. 예제
```
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="target">JavaScript</li>
</ul>
<input type="button" onclick="callReplaceChild();" value="replaceChild()" />
<script>
    function callReplaceChild(){
        var a = document.createElement('a');
        a.setAttribute('href', 'https://github.com/kwj1270');
        a.appendChild(document.createTextNode('Web browser JavaScript'));
 
        var target = document.getElementById('target');
        target.replaceChild(a,target.firstChild);
    }
</script>
```
```<li id="target">JavaScript</li>```의 첫번째 자식노드는 JavaScript이다.   
이것을 ```<a href='https://github.com/kwj1270'>Web browser JavaScript</a>```로 바꾼다는 뜻이다. 

***
# 4. JQuery 노드변경 API
> JQuery에서 노드를 제어하는 기능은 manipulation 카테고리에 속해있다.  
## 4.1. 추가
### 4.1.1. 추가 메소드
```
.before(node)           // 맨 앞 prepend보다 우선 순위로 앞이다.
.prepend(node)          // 맨 앞 
.append(node)           // 맨 뒤
.after(node)            // 맨 뒤 prepend보다 우선 순위로 앞이다.
```
![JQuery addMethod](https://user-images.githubusercontent.com/50267433/62006525-734d0800-b17c-11e9-81c2-feab57c9f3c5.png)
간단한 예제
```
<div class="target">
    content1
</div>
 
<div class="target">
    content2
</div>
 
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('.target').before('<div>before</div>');
    $('.target').after('<div>after</div>');
    $('.target').prepend('<div>prepend</div>');
    $('.target').append('<div>append</div>');
</script>
```
## 4.2. 제거
### 4.2.1. 제거 메소드
```
.remove()           // 자기 자신을 삭제 -> 하위 노드들은 상위 노드랑 연결이 끊겨지므로 삭제됨
.empty()            // 하위 노드들을 삭제 
```
간단한 예제
```
<div class="target" id="target1">
    target 1
</div>
 
<div class="target" id="target2">
    target 2
</div>
 
<input type="button" value="remove target 1" id="btn1" />
<input type="button" value="empty target 2" id="btn2" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#btn1').click(function(){
        $('#target1').remove();
    })
    $('#btn2').click(function(){
        $('#target2').empty();
    })
</script>
```
## 4.3. 바꾸기
### 4.3.1. 바꾸기 메소드
```
$(바뀌는대상).replaceWith(새로운 내용)
$(새로운 내용).replaceAll(바뀌는 대상)
```
간단한 예제
```
<div class="target" id="target1">
    target 1
</div>
 
<div class="target" id="target2">
    target 2
</div>
 
<input type="button" value="replaceAll target 1" id="btn1" />
<input type="button" value="replaceWith target 2" id="btn2" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#btn1').click(function(){
        $('<div>replaceAll</div>').replaceAll('#target1');
    })
    $('#btn2').click(function(){
        $('#target2').replaceWith('<div>replaceWith</div>');
    })
</script>
```
**복사**  
.clone()메소드를 이용한다.  
```
<div class="target" id="target1">
    target 1
</div>
 
<div class="target" id="target2">
    target 2
</div>
 
<div id="source">source</div>
 
<input type="button" value="clone replaceAll target 1" id="btn1" />
<input type="button" value="clone replaceWith target 2" id="btn2" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#btn1').click(function(){
        $('#source').clone().replaceAll('#target1');
    })
    $('#btn2').click(function(){
        $('#target2').replaceWith($('#source').clone());
    })
</script>
```
**이동**
append()같은 추가 메소드를 이용해서 기존에 존재하던 요소를   
특정 요소의 자식요소로 이동 시키는 것 같은 효과를 줄 수 있다.  
```
<div class="target" id="target1">
    target 1
</div>
 
<div id="source">source</div>
 
<input type="button" value="append source to target 1" id="btn1" />
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
    $('#btn1').click(function(){
        $('#target1').append($('#source'));
    })
</script>
```

***
# 5. 문자열로 노드 제어
## 5.1. 프로퍼티와 메소드
> 노드변경 API를 사용하는 방식은 복잡하고 장황하다.  
```
프로퍼티
innerHTML;                  // 자식 노드에 있는 하위 노드들을 문자열 형태로 반환 한다.
innerHTML = "새로운 값";     // 문자열로 자식 노드를 만든다 . 즉 "<p>text<p>"를 입력하면 해당 문자열처럼 노드가 생성된다.

outerHTML;                  // innerHTML()과 동작은 같으나 자기 자신도 같이 반환 한다.
outerHTML = "새로운 값";     // innerHTML("값")과 동작은 같으나 자식 노드가 아닌 자기 자신을 만드므로 기존 자기 자신도 사라진다.

innerText;                  // 문자열로 자식 노드의 Text객체의 값들을 가져온다.
innerText= "새로운 값";      // 문자열로 자식 노드의 Text객체의 값들을 바꾼다.  

outerText;                  // innerText()와 동작은 같지만 내부적으로 해당 태그도 포함한다(영향 x).
outerText = "새로운 값";     // innerText("값")와 동작은 같지만 해당 태그의 요소도 함께 삭제된다.

메소드
insertAdjacentHTML()        // 좀 더 정교한 문자열을 이용해서 노드를 변경하고 싶을 때 사용 
```
## 5.1.1. innerHTML예제
```
<ul id="target">
    <li>HTML</li>
    <li>CSS</li>
</ul>
<input type="button" onclick="get();" value="get" />
<input type="button" onclick="set();" value="set" />
<script>
    function get(){
        var target = document.getElementById('target');
        alert(target.innerHTML);
    }
    function set(){
        var target = document.getElementById('target');
        target.innerHTML = "<li>JavaScript Core</li><li>BOM</li><li>DOM</li>";
    }
</script>

```
## 5.1.2. outerHTML예제
```
<ul id="target">
    <li>HTML</li>
    <li>CSS</li>
</ul>
<input type="button" onclick="get();" value="get" />
<input type="button" onclick="set();" value="set" />
<script>
    function get(){
        var target = document.getElementById('target');
        alert(target.outerHTML);
    }
    function set(){
        var target = document.getElementById('target');
        target.outerHTML = "<ol><li>JavaScript Core</li><li>BOM</li><li>DOM</li></ol>";
    }
</script>
```

## 5.1.3. innerText, outerText 예제
```
<ul id="target">
    <li>HTML</li>
    <li>CSS</li>
</ul>
<input type="button" onclick="get();" value="get" />
<input type="button" onclick="set();" value="set" />
<script>
    function get(){
        var target = document.getElementById('target');
        alert(target.outerHTML);
    }
    function set(){
        var target = document.getElementById('target');
        target.outerHTML = "<ol><li>JavaScript Core</li><li>BOM</li><li>DOM</li></ol>";
    }
</script>
```
## 5.1.4. insertAdjacentHTML() 예제
```
<ul id="target">
    <li>CSS</li>
</ul>
<input type="button" onclick="beforebegin();" value="beforebegin" />
<input type="button" onclick="afterbegin();" value="afterbegin" />
<input type="button" onclick="beforeend();" value="beforeend" />
<input type="button" onclick="afterend();" value="afterend" />
<script>
    function beforebegin(){
        var target = document.getElementById('target');
        target.insertAdjacentHTML('beforebegin','<h1>Client Side</h1>');            // <ul>시작 전에
    }
    function afterbegin(){
        var target = document.getElementById('target');
        target.insertAdjacentHTML('afterbegin','<li>HTML</li>');                    // <ul>시작 후에
    }
    function beforeend(){
        var target = document.getElementById('target');
        target.insertAdjacentHTML('beforeend','<li>JavaScript</li>');               // <ul>끝에서 전에
    }
    function afterend(){
        var target = document.getElementById('target');
        target.insertAdjacentHTML('afterend','<h1>Server Side</h1>');               // <ul>끝에서 후에
    }
</script>
```
위 예제 같은 경우는 뒤로 해석하는 것이 좋다.  
```beforeend``` 가정시 ```end```의 ```before```이므로 끝나기 전에  
객체를 사용하는 경우 '객체가 끝나기 전에' 로 해석하는 것이 좋다.  
