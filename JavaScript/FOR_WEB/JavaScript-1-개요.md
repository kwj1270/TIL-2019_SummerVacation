웹브라우저와 javascript  
=======================
# 1. HTML
> HTML : HTML은 정보를 담아내고 표현하는 언어이다. 
## 1.1 초창기 웹의 모습
### 1.1.1. 웹 서버
```
정보를 저장 (웹 브라우저의 카운터 파트너)
```
### 1.1.2. 웹 브라우저
```
서버에 요청을 하고 응답 받는다.
```
### 1.1.3. 동작 원리
```
1. 웹 서버에 HTML이 저장 되어있고
2. 웹 브라우저가 HTML문서를 요청했을 때 
3. 웹 서버는 해당 HTML을 웹브라우저에 전달해준다.
  
이렇기 때문에 HTML은 웹에 있어서 가장 중요한 것이다. (웹의 시작이자 목적)
```
## 1.2. HTML코드
```
<!DOCTYPE html>
<html>
<body>
  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
  </ul>
</body>
</html>
```
  
***
# 2. CSS
> 정보를 디자인하고 꾸며주는 역할을 한다
  ## 2.1 CSS 코드
  ```
  <!DOCTYPE html>
<html>
<head>
    <style type="text/css">
        #selected{
            color:red;
        }
    </style>
</head>
<body>
    <ul>
        <li>HTML</li>
        <li>CSS</li>
        <li id="selected">JavaScript</li>
    </ul>
</body>
</html>
  ```
```
  <style type="text/css">
      #selected{
          color:red;
      }
  </style>
```
이쪽이 CSS 코드이다.

***
# 3. JavaScript
> HTML을 프로그래밍적으로 제어한다
## 3.1 JavaScript 코드
```
<!DOCTYPE html>
<html>
<head>
  <style type="text/css">
      #selected{
        color:red;
      }
      .dark {
        background-color:black;
        color:white;
      }
      .dark #selected{
        color:yellow;
      }
  </style>
</head>
<body>
  <ul>
      <li>HTML</li>
      <li>CSS</li>
      <li id="selected">JavaScript</li>
  </ul>
  <input type="button" onclick="document.body.className='dark'" value="dark" />
</body>
</html>
```
```
<input type="button" onclick="document.body.className='dark'" value="dark" />
에서 "document.body.className='dark'"
```
이쪽이 JavaScript 코드이다.
  

