 CASE 문으로 데이터 변환하기
=======================
# 1. CASE 문 
기존 프로그래밍 언어에서 많이 사용한 CASE를 DB에서도 사용할 수 있다
  
**구조**  
```
CASE [필드]  
     WHEN 조건식1 THEN 식1 
     [WHEN 조건식2 THEN 식2] 
     [ELSE 식3] 
END 
```
```CASE``` 에 필드가 있으면    
``WHEN```은 그 필드의 값을 통해서 새로운 식을 지정 할 수 있고ㅓ 
  
```CASE``` 에 필드가 없으면   
``WHEN```은 단순한 조건식을 통해서 새로운 식을 지정 할 수 있다.   
  
여기서 식이라는 것은 결과값을 의미하기도 한다.  
즉 ```WHEN``` 조건에 맞으면 THEN 이 실행된다.  
  
**예시**
```
mytable
__________
|a       |
|________|
|1       |
|________|
|2       |
|________|
|NULL    |
|________|

SELECT a,
CASE
       WHEN a IS NULL THEN 0 
       ELSE a 
END AS "a(null=0)" FROM mytable;   
```
**결과**
```
______________________
|a       | a(null=0) |
|________|___________|
|   1    |     1     |
|________|___________|
|   2    |     2     |
|________|___________|
|  NULL  |     0     |
|________|___________|
```
모든 행에 대하여 ```a``` 가 null 이면 0  
아니면 기존 ```a```의 값 출력이고  
이를 나타내는 필드를 "a(null=0)"로 나타내었다.
  
## 1.1. COALESCE()
CASE와 별개의 내용일 수 있겠지만  
NULL 값을 0으로 바꾸고자 한다면 ```COALESCE()```를 사용해도 된다.  
  
**예시**
```
SELECT a, COALESCE(a,0) FROM mytable;
```

***
# 2. 또 하나의 CASE 문 (디코드 인코드)
* 디코드 : 문자화
* 인코드 : 수치화
  
```CASE```는 이러한 디코드와 인코드를 사용하기 위해 많이 사용된다.  
  
**예시**
```
mytable
__________
|a       |
|________|
|1       |
|________|
|2       |
|________|
|NULL    |
|________|

SELECT a AS "코드",
CASE 
       WHEN a=1 THEN '남자' 
       WHEN a=2 THEN '여자' 
       ELSE '미지정'
END AS "성별" FROM mytable;   
```

***
# 3. CASE를 사용할 경우 주의사항
## 3.1. ELSE 생략
```ELSE```를 생략할경우 자동적으로 
```
ELSE NULL
```
이 된다.
NULL은 DB에서는 어떠한 연산도 할 수 없으므로 이를 주의해야하고   
되도록 ELSE를 생략하지 말고 지정하는 편이 좋다.

## 3.2. WHEN에서 NULL 지정하기
  
**예시**
```
SELECT a,
CASE a
       WHEN 1 THEN '남자' 
       WHEN 2 THEN '여자' 
       WHEN NULL THEN '데이터 없음'   
       ELSE '미지정'
END AS "성별" FROM mytable;   
```
```WHEN NULL```은 단순히 문법적으로는 문제가 없지만 정상적으로 처리되지 않는다.   
사실은 ```WHEN``` 구문이 ```WHEN a = NULL```로 조건식을 처리하기 때문이다.   
***NULL***은 대입 연산자를 제외한 어떠한 연산도 불가능 하므로 판정이 불가능하다.  
여기서 나온 ```=```은 ```equal``` 의미이다. 
  
DB에서 NULL을 판정하기 위해서는 ```IS NULL```을 사용한다.  
  
**해결**
```
SELECT a,
CASE 
       WHEN a=1 THEN '남자' 
       WHEN a=2 THEN '여자' 
       WHEN a IS NULL THEN '데이터 없음'   
       ELSE '미지정'
END AS "성별" FROM mytable;   
```

