SQL 기본(02)
=======================
# 1. GROUP BY 및 HAVING 그리고 집계 함수
## 1.1. GROUP BY 절
**형식**
```
 SELECT [ALL | DISTINCT] 컬럼명 [,컬럼명...]
    FROM 테이블명 [,테이블명...]
    [WHERE 조건식]
->  [GROUP BY {col_name | expr | position}]
->  [HAVING where_condition]
    [ORDER BY {col_name | expr | position}]
```
```GROUP BY```는 그룹을 묶어주는 역할을 한다.  
  
**예시**
```
SELECT userID, amount FROM buyTbl GROUP BY userID;
```
위 예제의 결과는 사용자 별로 각각의 행이 별도로 출력된다 단 중복되어서 나온다.  
만약에 사용자별로 중복된 데이터를 하나로 모으고 싶으면 **집계함수**를 사용한다.   


**예시**
```
SELECT userID AS '사용자아이디', SUM(amount) AS '총 구매 개수' FROM buyTbl GROUP BY userID;
```
위 예제의 결과는 사용자 별로 각각의 행이 별도로 출력되며 중복되었던 값들을 다 더해서  
중복이 없는 결과 데이터를 출력한다.  
  
만약 GROUP BY를 사용하지 않았을 경우 SUM(amount)는 기준없이 모든 amount의 값을 더했을 것이다.  
이렇듯 GROUP BY는 어떠한 기준을 만들어주는 것이다.
그리고 ```SUM(price * amount)``` 이런식으로 괄호안에 연산을 해도 된다.  

## 1.2. 집계함수
```
AVG()                 :       평균을 구한다
MIN()                 :       최소값을 구한다
MAX()                 :       최대값을 구한다
COUNT()               :       행의 개수를 센다
COUNT(DISTINCT)       :       행의 개수를 센다 (중복시 1개만 인정) 
STDEV()               :       표준편차를 구한다 
VAR_SAMP()            :       분산을 구한다.
```
각 상황에 맞춰서 쓰자

## 1.3. HAVING 절
HAVING 절은 집계함수에 대해서 조건을 제한하는 것이다.  
즉 집계함수를 위한 WHERE 구문이라 보면 된다. (집계함수를 WHERE에 사용하면 에러발생)  
  
**구조**
```
HAVING 조건식
```
  
**예시**
```
SELECT userID AS '사용자아이디',
       SUM(price * amount) AS "총 구매 금액" 
       FROM buyTbl 
       GROUP BY userID;
       HAVING SUM(price * amount) > 10000
       ORDER BY SUM(price * amount);
```
추가로 ```ORDER BY```에서도 집계함수를 사용 할 수 있다.

## 1.4. ROLLUP
분류별로 합계를 구해주는 명령어다.  
주로 중간합계를 구할 때 사용된다.   
  
**예시**
```
SELECT groupNme,
       SUM(price * amount) AS "비용" 
       FROM buyTbl 
       GROUP BY groupName , num ;
       WITH ROLLUP;
```
위 예제의 결과는 각 분류마다  
즉 groupName 마다 소합계가 나타나며 
마지막에는 각각의 소합계를 더한 총 합계가 나타난다.
