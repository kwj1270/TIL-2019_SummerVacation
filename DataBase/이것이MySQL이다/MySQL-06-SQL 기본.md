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

### 1.2.1 WHERE 와 관계연산자 
```
AND   : &&와 같은 의미 양쪽 피연산자의 값이 TRUE 이어야 한다.
OR    : ||와 같은 의미 양쪽 피연산자 중에 하나라도 값이 TRUE 이면 된다.
NOT   : !와 같은 의미 TRUE -> FALSE / FALSE -> TRUE

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
