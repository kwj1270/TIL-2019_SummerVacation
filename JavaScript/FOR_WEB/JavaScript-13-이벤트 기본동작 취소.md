Event 기본 동작의 취소
=======================
웹 브라우저의 구성요소들은 각각 기본적인 동작 방법을 가지고 있다.
 * 텍스트 필드에 포커스를 준 상태에서 키보드를 입력하면 텍스트가 입력된다.
 * 폼에서 submit 버튼을 누르면 데이터가 전송된다.
 * ```<a>``` 태그를 클릭하면 href 속성의 URL로 이동한다.  
   
간략하게 설명하면 이벤트 동작 외에
태그가 가지고 있던 기본 동작을 말하는 것이다.

***
# 1. INLINE 
> 이벤트의 리턴값이 false이면 기본 동작이 취소된다.
## 1.1. 코드
```
<p>
    <label>prevent event on</label><input id="prevent" type="checkbox" name="eventprevent" value="on" />
</p>
<p>
    <a href="http://opentutorials.org" onclick ="if(document.getElementById('prevent').checked) return false;">opentutorials</a>
</p>
<p>
    <form action="http://opentutorials.org" onsubmit="if(document.getElementById('prevent').checked) return false;">
            <input type="submit" />
    </form>
</p>
```
핸들러의 return 값이 false 일 경우에는 기본 동작을 취소한다.

***
# 2. PROPERTY 방식
> 이벤트의 리턴값이 false이면 기본 동작이 취소된다.
## 2.1. 코드
```
<p>
    <label>prevent event on</label><input id="prevent" type="checkbox" name="eventprevent" value="on" />
</p>
<p>
    <a href="http://opentutorials.org">opentutorials</a>
</p>
<p>
    <form action="http://opentutorials.org">
            <input type="submit" />
    </form>
</p>
<script>
    document.querySelector('a').onclick = function(event){
        if(document.getElementById('prevent').checked)
            return false;
    };
     
    document.querySelector('form').onclick = function(event){
        if(document.getElementById('prevent').checked)
            return false;
    };
 
</script>
```
property 방식은 Inline 방식과 다르게 function을 사용하는데  
이 function의 리턴 값이 false 이면 기본 동작이 취소된다. 

***
# 3. addEventListener
## 3.1. 코드
```
<p>
            <label>prevent event on</label><input id="prevent" type="checkbox" name="eventprevent" value="on" />
        </p>
        <p>
            <a href="http://opentutorials.org">opentutorials</a>
        </p>
        <p>
            <form action="http://opentutorials.org">
                    <input type="submit" />
            </form>
        </p>
        <script>
            document.querySelector('a').addEventListener('click', function(event){
                if(document.getElementById('prevent').checked)
                    event.preventDefault();
            });
             
            document.querySelector('form').addEventListener('submit', function(event){
                if(document.getElementById('prevent').checked)
                    event.preventDefault();
            });
 
        </script>
```
addEventListener 방식은 기존 false를 리턴하는 것과 달리  
event 객체의 **preventDefault()** 를 실행하는 것이다.
```
('click', function(event){
      if(document.getElementById('prevent').checked) event.preventDefault();
}
```            
또한 IE9 이하 버전에서는 ```event.returnValue = false```로 해야 한다. 
