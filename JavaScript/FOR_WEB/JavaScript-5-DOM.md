DOM(Document OBject Model)
=======================
> 웹페이지(HTML)를 javascript로 제어하기 위한 객체 모델  
> Document 객체의 프로퍼티는 문서내의 주요 엘리먼트에 접근 할 수 있는 객체를 제공

# 1. 제어대상 찾기
## 1.1. getElementByTagName()
```
getElementByTagName('태그');
getElementByTagName('body');
```
태그를 기준으로 해당 요소의 객체를 얻는다.  
모든 요소를 유사배열에 담는다.(1개라도 예외x)  
  
만약 조회 대상을 좁히고 싶으면 아래와 같이 사용한다.
```
var ul = document.getElementsByTagName('ul');
var lis = ul.getElementsByTagName('li');
```
## 1.2. getElementByClassName()
```
getElementByClassName('클래스명');
getElementByClassName('glass');
```
클래스를 기준으로 해당 요소의 객체를 얻는다.  
모든 요소를 유사배열에 담는다. (1개라도 예외x) 
선택자가 아니므로 .을 붙이지는 않는다.
## 1.3. getElementById()
```
getElementById('아이디명');
getElementById('yd');
```
아이디를 기준으로 해당 요소의 객체를 얻는다.  
id 는 1회성으로 사용되므로 배열에 담지 않은 객체를 얻는다. 
선택자가 아니므로 #을 붙이지는 않는다.
## 1.4. querySelector()
```
querySelector('선택자');
querySelector('태그');
querySelector('.클래스');
querySelector('#아이디');
```
선택자를 기준으로 해당 요소의 객체를 얻는다.  
선택자이므로 .(class) / #(id) 을 사용해야 하며
CSS 선택자 문법을 그대로 이용하면 된다.  
단 선택된 요소가 여러개일 경우 선택한 조건에 맞는 가장 가까운 요소의 객체를 얻는다.

```
vat li = documnet.querySelector('li');
li.style.color = "blue";
가장 맨위의 li의 폰트색상이 파랑색이 된다.
```
## 1.5. querySelectorAll()
```
querySelectorAll('선택자');
querySelectorAll('태그');
querySelectorAll('.클래스');
querySelectorAll('#아이디');
```
선택자를 기준으로 해당 요소의 객체를 얻는다.  
선택자이므로 .(class) / #(id) 을 사용해야 하며
CSS 선택자 문법을 그대로 이용하면 된다.
querySelector와 다르게 모든 요소를 유사배열에 담는다.
```
vat li = documnet.querySelector('li');
li.style.color = "blue";
모든 li의 폰트색상이 파랑색이 된다.
```

***
# 2. HTML Element
> 유사 배열이 아닌 하나의 요소가 담긴 객체
## 2.1. HTMLLIElement
```<li>```로부터 얻는 객체  

```
<li id = 'list'>~ </li>
var target = document.getById('list');
console.log(target.constructor.name);
```
## 2.2. HTMLAnchorElement
```<a>```로부터 얻는 객체

```
<a id = 'anchor'>~ </a>

var target = document.getById('anchor');
console.log(target.constructor.name);
```
## 2.3. HTMLInputElement
```<input>```로부터 얻는 객체

```
<input type="button" id = 'btn' />

var target = document.getById('btn');
console.log(target.constructor.name);
```   

***
# 3. HTMLCollection
> get 메소드시 유사배열 형태로 대상을 담아서 반환될 때 반환되는 객체의 이름  
> 특이점으로는 배열(목록)이 실시간으로 변경 된다.
## 3.1. 코드
```
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="active">JavaScript</li>
</ul>
<script>
console.group('before');
var lis = document.getElementsByTagName('li');
for(var i = 0; i < lis.length; i++){
    console.log(lis[i]);
}

/////////////////분기점/////////////////

console.groupEnd();
console.group('after');
lis[1].parentNode.removeChild(lis[1]);            -> 인덱스 값을 삭제한다.
for(var i = 0; i < lis.length; i++){
    console.log(lis[i]);
}
console.groupEnd();
</script>
</body>
</html>
```
일반적인 변수의 시선으로 보면 차이점을 못 느끼겠지만  
노드에 변경사항이 있는 경우에 그 변경사항에 담긴 HTMLCollection 데이터에서도 함께 반영된다.

***
# 4. Element 객체
> 모든 HTML의 Element 들은 HTMLElement의 자식이다.  
  
Dom은 HTML 뿐만 아니라 XML,SVG,XUL과 같은  
마크업 형태의 언어를 제어하기 위한 규격이다.

Element는 마크업 언어의 일반적인 규격에 대한 속성을 정의하고 있고  
각각의 구체적인 언어를 위한 기능은 각각의 ~Element 객체를 통해서 추가해서 사용하고 있다.
## 4.1. 식별자 API
### 4.1.1. tagName
```
<ul>
    <li>html</li>
    <li>css</li>
    <li id="active" class="important current">JavaScript</li>
</ul>
<script>
console.log(document.getElementById('active').tagName)
</script>
```
태그의 이름을 얻을 수 있다. 단, 변경은 불가능하다.
### 4.1.1. id
```
<ul>
    <li>html</li>
    <li>css</li>
    <li id="active">JavaScript</li>
</ul>
<script>
var active = document.getElementById('active');
console.log(active.id);
active.id = 'deactive';
console.log(active.id);
</script>
```
id의 값을 얻을 수 있고 변경 또한 가능하다.
### 4.1.1. className
```
<ul>
    <li>html</li>
    <li>css</li>
    <li id="active">JavaScript</li>
</ul>
<script>
var active = document.getElementById('active');
// class 값을 변경할 때는 프로퍼티의 이름으로 className을 사용한다.
active.className = "important current";
console.log(active.className);
// 클래스를 추가할 때는 아래와 같이 문자열의 더한다.
active.className += " readed"
</script>
```
class의 값을 얻을 수 있고 변경 또한 가능하다.
단 className보다 classList를 추천한다.
### 4.1.1. classList
```
<ul>
    <li>html</li>
    <li>css</li>
    <li id="active" class="important current">JavaScript</li>
</ul>
<script>
function loop(){
    for(var i=0; i<active.classList.length; i++){
        console.log(i, active.classList[i]);
    }
}
// 클래스를 추가
</script>
<input type="button" value="DOMTokenList" onclick="console.log(active.classList);" />
<input type="button" value="조회" onclick="loop();" />
<input type="button" value="추가" onclick="active.classList.add('marked');" />
<input type="button" value="제거" onclick="active.classList.remove('important');" />
<input type="button" value="토글" onclick="active.classList.toggle('current');" />
```
classList를 사용하면 
```<li id="active" class="important current">JavaScript</li>```  
클래스이름의 important 와 current를 유사배열에 담아서 
```function loop(){
    for(var i=0; i<active.classList.length; i++){
        console.log(i, active.classList[i]);
    }
```
반복문 및 add,remove,toggle 메소드를 통해 클래스의 값을 제어 할 수 있다.
## 4.2. 조회 API
### 4.2.1. 대상과 제한
get~ 및 query~는 위에 참조  
  
내가 현재 찾고자 하는 대상이 특정 요소의 하위 객체라면  
getElement(s)By~ 메소드를 사용하여 조회 대상을 제한 할 수 있다.

```
<ul>
    <li class="marked">html</li>
    <li>css</li>
    <li id="active">JavaScript
        <ul>
            <li>JavaScript Core</li>
            <li class="marked">DOM</li>
            <li class="marked">BOM</li>
        </ul>
    </li>
</ul>
<script>
    var list = document.getElementsByClassName('marked');
    console.group('document');
    for(var i=0; i<list.length; i++){
        console.log(list[i].textContent);
    }
    console.groupEnd();
     
    console.group('active');
    var active = document.getElementById('active');     
    var list = active.getElementsByClassName('marked');
    for(var i=0; i<list.length; i++){
        console.log(list[i].textContent);
    }
    console.groupEnd();
</script>
```
주의점 : getElement(s)By~ 메소드는 **하위요소**만 가져온다  
즉, 부모 형제는 적용이 되지 않는다.
## 4.3. 속성 API
### 4.3.1. 속성 방식
```
Element.getAttribute(name)            속성 값 가져오기
Element.setAttribute(name, value)     속성 값 설정하기
Element.hasAttribute(name);           속성 값 존재 확인
Element.removeAttribute(name);        속성 없애기
```
```
<a id="target" href="https://github.com/kwj1270">kwj1270</a>
<script>
var t = document.getElementById('target');
console.log(t.getAttribute('href'));                //"https://github.com/kwj1270"
t.setAttribute('title', 'opentutorials.org');       // title 속성의 값을 설정한다.
console.log(t.hasAttribute('title'));               // true, title 속성의 존재여부를 확인한다.
t.removeAttribute('title');                         // title 속성을 제거한다.
console.log(t.hasAttribute('title'));               // false, title 속성의 존재여부를 확인한다.
</script>
```
### 4.3.1. 속성 방식과 프로퍼티 방식 비교
```
p id="target">
    Hello world
</p>
<script>
    var target = document.getElementById('target');      
    target.setAttribute('class', 'important');           // attribute 방식
    target.className = 'important';                      // property 방식
</script>
```
```
속성            프로퍼티
class          className
readonly       readOnly
rowspan        rowSpan
colspan        colSpan
frameborder    frameBorder
for            htmlFor
maxlentgh      maxLength
```
모든 엘리먼트들은 속성과 프로퍼티로 제어가 가능하다.

```
<a id="target" href="./demo1.html">ot</a>
<script>
//현재 웹페이지가 http://localhost/webjs/Element/attribute_api/demo3.html 일 때 
var target = document.getElementById('target');
console.log('target.getAttribute("href")', target.getAttribute("href"));  //상대 경로 출력
console.log('target.href', target.href);                                  //절대 경로 출력
</script>
```

속성 방식 : 상대경로를 출력한다.  
프로퍼티 방식 : 절대 경로를 출력한다.  

