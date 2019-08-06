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
