DOM(Document OBject Model)
=======================
> 웹페이지(HTML)를 javascript로 제어하기 위한 객체 모델  
> Document 객체의 프로퍼티는 문서내의 주요 엘리먼트에 접근 할 수 있는 객체를 제공

# 1. 제어대상 찾기
> 문서를 제어하기 위한 필수 행위이다.
## 1.1 getElementByTagName()
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
단 선택된 요소가 여러개일 경우
선택한 조건에 맞는 가장 가까운 요소의 객체를 얻는다.

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
<li>로부터 얻는 객체

```
<li id = 'list'>~ </li>

var target = document.getById('list');
console.log(target.constructor.name);

```
## 2.2. HTMLAnchorElement
<a>로부터 얻는 객체

```
<a id = 'anchor'>~ </a>

var target = document.getById('anchor');
console.log(target.constructor.name);
```

## 2.3. HTMLInputElement
<input>로부터 얻는 객체

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

