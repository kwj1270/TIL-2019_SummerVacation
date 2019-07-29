EVENT
=======================
> 어떠한 동작이나 사건을 의미  

**자바스크립트 이벤트 구성요소**  

```
1. event target           // 이벤트 대상을 의미  
2. event type             // 이벤트 종류
3. event handler          // 이벤트 동작(행동)
```

# 1. 이벤트 등록 방법
## 1.1 INLINE
> 이벤트 대상의 태그속성을 이용하여 이벤트 등록 
### 1.1.1. 기본 방식
```
<input type = "button" oncilck = "alert('hello world');" value = "button"/>
               대상       종류         동작
```
이벤트 대상 : button  
이벤트 종류 : onclick  
이벤트 동작 : alert('hello world')이다.  
  
즉 button이 click 되면  alert('hello world')을 발생시켜라 라는 의미가 된다.
### 1.1.2. 자기 자신 이벤트 등록
**불편한 방법** 
```
<input type="button" id="target" onclick="alert('Hello world, '+document.getElementById('target').value);" value="button" />
```
**this를 활용한 방법**
```
<input type="button" onclick="alert('Hello world, '+this.value);" value="button" />
```
this 객체는 원래는 자기 자신을 호출한 대상을 의미하는데(일반적으로 window 객체안에서는 객체 의미)  
이벤트에서는 이 this를 event target 즉 이벤트 대상으로 변경시켰다.   
그래서 이벤트 대상을 의미하는 this 객체를 이용하면 조금더 간편하게 코드를 작성 할 수 있다.  
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
