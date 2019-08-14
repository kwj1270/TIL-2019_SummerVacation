SQL 기본
=======================
DB를 사용하기 위한 코드
1. ```CREATE DATABASE 데이터베이스명 ;``` 데이터베이스(스키마) 생성
2. ```USE 데이터베이스명 ;``` 사용할 데이터베이스(스키마) 지정
3. ```CREATE TABLE 테이블명(생성코드) ;``` 테이블 생성
4. ```SHOW TABLES ;``` 테이블 확인 또는 ```SHOW TABLES STATTUS```
5. ```DESC 테이블명 ;``` 특정 테이블 구조 확인
  
위 코드들은 기본으로 사용하는 코드이므로 외워두자
# 1. SELECT
## 1.1. SELECT...FROM 
### 1.1.1. SELECT 구문형식
**기본 구문**
```
 SELECT [ALL | DISTINCT] 컬럼명 [,컬럼명...]
 FROM 테이블명 [,테이블명...]
 [WHERE 조건식]
 [GROUP BY 컬럼명 [HAVING 조건식]]
 [ORDER BY 컬럼명]
 GROUP BY 컬럼명[,컬럼명...]
 ORDER BY 컬럼명[,컬럼명...]
```
[]는 생략가능 의미  
  
**간략**
```
SELECT 열(필드)
FROM 테이블
WHERE 조건
```
### 1.1.2. SELECT 와 FROM
**예시**
```
SELECT * FROM titles;
SELECT id,name FROM titles;
```
* ```*```는 전체를 의미 즉 모든 필드(열)를 의미한다.
* 일부만 사용하고자 한다면 필드명을 써주면 된다. 여러개일 경우 ```,```사용

## 1.2. SELECT...FROM...WHERE 조건 
**기본 구조**
```
SELECT 열 FROM 테이블 WHERE 조건식;
```
**예시**
```
SELECT * FROM mytable WHERE id >= 10;
```
id가 10 이상인 데이터(행) 조회

### 1.2.1. WHERE 와 관계연산자 
```
AND   : &&와 같은 의미 양쪽 피연산자의 값이 TRUE 이어야 한다.
OR    : ||와 같은 의미 양쪽 피연산자 중에 하나라도 값이 TRUE 이면 된다.
NOT   : !와 같은 의미 TRUE -> FALSE / FALSE -> TRUE
```
```
SELECT name FROM mytable WHERE birthYear >= 1970 AND height >= 182;
SELECT name FROM mytable WHERE birthYear >= 1970 OR height >= 182;
SELECT name FROM mytable WHERE NOT(birthYear >= 1970 AND height >= 182);
```
```AND``` 와 ```OR``` 는 양쪽에 피연산자가 있지만  
```NOT```은 한개의 피연산자만 있다고 생각을 하면 된다.

### 1.2.2. BETWEEN...AND 와 IN() 그리고 LIKE
**BETWEEN...AND**
```
SELECT name FROM mytable WHERE height >= 170 AND height <= 182;
```
위 예제와 같이 동일한 필드를 기준으로   
숫자로 구성되어 연속적인 값을 사용하는 경우는 ```BETWEEN...AND```을 사용할 수 있다. 
```
SELECT name FROM mytable WHERE height BETWEEN 170 AND 182;
```
**IN()**
```
SELECT name FROM mytable WHERE addr='경남' OR addr=전남'' OR addr='충남';
```
위 예제와 같이 동일한 필드를 기준으로   
문자로 구성되어 이산적인 값을 사용하는 경우는 ```IN()```을 사용할 수 있다. 
```
SELECT name FROM mytable WHERE addr IN('경남','전남','충남');
```
**LIKE**
문자열의 내용을 검색하기 위해서는 LIKE 연산자를 사용할 수 있다.
```
SELECT name FROM mytable WHERE name LIKE '홍%';
```
```%```는 무엇이든 허용한다는 의미이다.(문자열)
```%길동``` 이렇게 앞에 사용한다던가  
```%길%``` 이렇게 양쪽에 사용 가능하다.
```
SELECT name FROM mytable WHERE name LIKE '_길동';
```
```_```는 한개의 글자를 허용한다는 의미이다.(문자)
```_길동``` 이렇게 앞에 사용한다던가  
```_길_``` 이렇게 양쪽에 사용 가능하다.

### 1.2.3. ANY/ALL/SOME 그리고 서브쿼리(하위쿼리)
```
SELECT Name, height FROM userTbl
  WHERE hegiht > (SELECT height FROM userTbl WHERE Name = '김경호')
```
위의 코드를 보면 김경호의 키보다 큰 사람의 ```Name```과 ```height```를 출력하는 것을 알 수 있다.   
위의 코드가 정상작동을 하기 위한 조건은 김경호는 1명일 경우를 가정한 것이다.  
그렇다면 만약 김경호가 2명 이상이라면 그 조건은 어떻게 되는가?  
일단 DB에서 이러한 상황에 맞딱드릴 경우 ERROR를 발생시킨다.   
바로 이럴 때 사용하는 것이 **ANY/ALL/SOME**이다.  
  
**ANY 또는 SOME**
```
김경호(1)이 170 
김경호(2)가 173이라 가정

SELECT Name, height FROM userTbl
  WHERE hegiht > ANY(SELECT height FROM userTbl WHERE Name = '김경호')

SELECT Name, height FROM userTbl
  WHERE hegiht > SOME(SELECT height FROM userTbl WHERE Name = '김경호')
```
```
SELECT Name, height FROM userTbl
  WHERE hegiht > 170
```
```ANY/SOME```은 2가지 중 ```( > 170)/( > 173)```
하나라도 만족하면 되기에 작은수 170이 기준이 된다.  

**ALL**
```
김경호(1)이 170 
김경호(2)가 173이라 가정

SELECT Name, height FROM userTbl
  WHERE hegiht > ALL(SELECT height FROM userTbl WHERE Name = '김경호')
```
```
SELECT Name, height FROM userTbl
  WHERE hegiht > 173
```
```ALL```은 2가지 중 ```( > 170)/( > 173)```
모든 조건을 만족해야 되기에 작은수 173이 기준이 된다.  

이렇듯 여러 값이 반환될때 사용하는 것이 **ANY/ALL/SOME**이다

## 1.3. 원하는 순서대로 정렬하기 OREDER BY
```ORDER BY``` 절은 데이터에 직접적인 영향을 미치지는 않지만   
결과가 출력되는 순서를 조절해주는 구문이다. 즉 실제 데이터 순서에는 영향을 미치지 않는다.  
  
**예시**
```
SELECT Name, mDate FROM userTbl OREDER BY mDate ASC;
SELECT Name, mDate FROM userTbl OREDER BY mDate DESC;
```
```ORDER BY 열``` 의 열을 기준으로
```ASC```는 오름차순
```DESC```는 내림차순으로 정렬을 한다.

## 1.4. 중복된 것은 하나만 남기는 DISTINCT
```DISTINCT```는 주로 ```SELECT```구문에 쓰이며 중복된 값중 한개만 보여준다.
  
**예시**
```
SELECT DISTINCT addr FROM userTbl;
```

## 1.5. 출력의 개수를 제한하는 LIMIT
```LIMIT```은 행 개수를 제한해주는 명령어이다.  
```
1. 

SELECT emp_no, hire_date FROM employees
  ORDER BY hire_date ASC
  LIMIT 5;
  
=========================================  
2.

SELECT emp_no, hire_date FROM employees
  ORDER BY hire_date ASC
  LIMIT 0,5;

=========================================  
3.

SELECT emp_no, hire_date FROM employees
  ORDER BY hire_date ASC
  LIMIT 5 OFFSET 0;
```
```1.```은 단순한 LIMIT 사용이며 5개로 제한을 의미한다.  
```2.```는 두개의 값을 사용한 LIMIT 사용이며 0개 부터 시작하여 5개로 제한을 의미한다.  
```3.```도 LIMIT 사용하여 0개 부터 시작하여 5개로 제한을 의미한다.  
```2.```와 ```3.```은 같은 표현이다.   
이렇게 사용하는 이유는 엄청나게 많은 데이터를 페이지를 이용하여 편리하게 사용할 수 있기 위해서이다.  

## 1.6. 테이블을 복사하는 CREATE TABLE ...SELECT
``` CREATE TABLE ...SELECT```는 주로 복사할 경우 주로 사용된다.  
  
**구조**
```
CREATE TABLE 새로운 테이블 (SLECT 복사할 필드 FROM 기존 테이블 WHERE 조건식)
```
**예시**
```
CREATE TABLE newtable (SLECT * FROM mytable);
```
물론 열을 지정할 수 있고
```
CREATE TABLE newtable (SLECT id, name , age FROM mytable);
```
WHERE 조건식 을 지정할수도 있다.
```
CREATE TABLE newtable (SLECT id, name , age FROM mytable WHERE id = 1);
```
