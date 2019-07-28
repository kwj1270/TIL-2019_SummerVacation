Text 객체
=======================
> Text객체는 Text 노드에 대한 DOM 객체로 character Data를 상속 받는다.  

DOM에서는 공백이나 줄바꿈도 텍스트 노드로 칭한다.
```
<p id="target1"><span>Hello world</span></p>
<p id="target2">
    <span>Hello world</span>
</p>
<script>
var t1 = document.getElementById('target1').firstChild;
var t2 = document.getElementById('target2').firstChild;
 
console.log(t1.firstChild.nodeValue);
try{
    console.log(t2.firstChild.nodeValue);   
} catch(e){
    console.log(e);
}
console.log(t2.nextSibling.firstChild.nodeValue);
 
</script>
```
위 예제를 보면 t2는 줄바꿈으로 인한 Text 노드로 인해서  
예외처리 Try...Catch를 한 것을 볼 수 있다.


# 1. 값 API
> Text노드는 DOM에서 실질적인 데이터가 저장되는 객체이다.
## 1.1. 프로퍼티
```
사용 대상은 Text 객체이고
텍스트 객체의 내용을 보여준다.

nodeValue;                 // 텍스트 객체의 내용을 가져온다 
nodeValue = "새로운값";     // 텍스트 객체의 내용을 새로이 쓴다.

data;                      // 읽기만 가능
```
### 1.1.1 예제
```
<ul>
    <li id="target">html</li> 
    <li>css</li>
    <li>JavaScript</li>
</ul>
<script>
    var t = document.getElementById('target').firstChild;
    console.log(t.nodeValue);
    console.log(t.data);
</script>
```

***
# 2. 조작 API
> Text 노드가 상속받은 character Data 객체는 문자를 제어할수 있다
## 2.1. 메소드
```
appendData(값)               // 뒤에 입력
deleteData(idx, 개수)        // idx 부터 개수 만큼 삭제
insertData(idx, 값)          // idx 부터 값 입력 (기존 값은 뒤로)
replaceData(idx, 개수, 값)    // idx 부터 개수 만큼 삭제 후 값 넣기
substringData(idx, 개수)      // idx 부터 개수 만큼을 리턴
```  
### 2.1.1. 예제
```
<!DOCTYPE html>
<html>
<head>
    <style>
    #target{
        font-size:77px;
        font-family: georgia;
        border-bottom:1px solid black;
        padding-bottom:10px;
    }
    p{
        margin:5px;
    }
    </style>
</head>
<body>
<p id="target">Cording everybody!</p>
<p> data : <input type="text" id="datasource" value="JavaScript" /></p>
<p>   start :<input type="text" id="start" value="5" /></p>
<p> end : <input type="text" id="end" value="5" /></p>
<p><input type="button" value="appendData(data)" onclick="callAppendData()" />
<input type="button" value="deleteData(start,end)" onclick="callDeleteData()" />
<input type="button" value="insertData(start,data)" onclick="callInsertData()" />
<input type="button" value="replaceData(start,end,data)" onclick="callReplaceData()" />
<input type="button" value="substringData(start,end)" onclick="callSubstringData()" /></p>
<script>
    var target = document.getElementById('target').firstChild;
    var data = document.getElementById('datasource');
    var start = document.getElementById('start');
    var end = document.getElementById('end');
    function callAppendData(){
        target.appendData(data.value);
    }
    function callDeleteData(){
        target.deleteData(start.value, end.value);
    }
    function callInsertData(){
        target.insertData(start.value, data.value); 
    }
    function callReplaceData(){
        target.replaceData(start.value, end.value, data.value);
    }
    function callSubstringData(){
        alert(target.substringData(start.value, end.value));
    }
</script>
</body>
</html>
```
