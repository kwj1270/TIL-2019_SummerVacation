객체 모델
=======================
> Window  
> DOM  
> BOM  

웹브라우저의 구성요소들은 하나하나 객체화 되어 있다.  
이러한 객체들은 서로 계층적인 관계로 구조화 되어 있으며  
**BOM** 과 **DOM** 은 이 구조로 구성하고 있는 가장 큰 틀의 분류이다.  

이 관계를 그림으로 나타내면 아래와 같다. (출처 : http://learn.javascript.ru/browser-environment)
![JavaScript-Objectmodel](https://user-images.githubusercontent.com/50267433/62001610-39a1e000-b12f-11e9-99b6-8b2c611b97bd.png)

웹브라우저는 HTML 문서를 쉽게 읽어 웹페이지를 만드는 동시에  
HTML의 각 태그들을 개체로 만들어 놓는다.  
JavaScript는 이러한 객체들을 통해 웹 페이지를 조작,제어할 수 있다.

# 1. Window 객체
## 1.1 의미
```
1. 전역 객체
2. window/frame 을 제어하기 위한 객체 (브라우저 창)
```

***
# 2. DOM
## 2.1 의미
```
웹페이지의 내용 객체 -> 즉, HTML문서 객체
```
Document 객체가 대표적이다.  
Documnet 객체의 프로퍼티는 문서내의 주요 엘리먼트에 접근할 수 있다.

***
# 3. BOM
## 3.1 의미
```
웹 브라우저의 각종 요소들에 대한 객체 (DOM제외)
웹 브라우저에 대한 정보
```
