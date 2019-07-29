Document 객체
=======================
> Document 객체는 DOM의 스팩이고 이것이 웹브라우저에서는 HTMLDocument 객체로 사용된다.  
> HTMLDocument 객체는 문서 전체를 대표하는 객체라고 할 수 있다.  
1. document 객체는 window 객체의 소속이다.
2. document 객체는 자식으로는 Doctype 과 html이 있다. ```<!DOCTYPE>, <html>```
3. Node는 DOM 구조의 시조 객체이므로 document도 Node API를 사용 가능하다.  
  
# 1. 노드 생성 API
## 1.1. 메소드
```
createElement()             // 엘리멘트 생성 (태그)
createTextNode()            // Text 노드를 생성    
```

# 2. 문서 정보 API
## 2.1. 프로퍼티
```
document.title          // 문서의 title 값
        .URL            // 문서의 URL 값
        .referrer       // 경유 사이트
        .lastModified   // 최신 갱신(수정) 시간
```   

