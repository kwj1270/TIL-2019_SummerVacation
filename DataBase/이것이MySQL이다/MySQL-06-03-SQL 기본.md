SQL 기본 (3)
=======================
# 1. 데이터의 삽입 : INSERT

## 1.1. INSERT문의 기본
**구조**
```
INSERT [INTO] 테이블 [(열1, 열2, ...)] VALUES (값1, 값2, ...)
```
SELECT에서 주의해야할 점은  
```[(열1, 열2, ...)]```을 생략하면 ```VALUES```에 모든 열의 값을 넣어 주어야한다.   

## 1.2. 자동으로 증가하는 AUTO_INCREMENT
테이블의 속성이 ```AUTO_INCREMENT```로 지정 되어 있다면 
값을 입력해주지 않아도 자동으로 1씩 증가된다 
```
id가 AUTO_INCREMENT라는 가정하에 

INSERT INTO mytable(id,name,age) VALUES (NULL, 'kim', 24)
```
```NULL값```을 넣어주어도 ```AUTO_INCREMENT```로 인해서 기존값에서 1 증가된 값으로 변한다.  
  
**추가 1**
```
SELECT LAST_INSERT_ID();
```
마지막에 입력된 값을 보여준다.  
  
**추가 2**
```
ALTER TABLE mytable AUTO_INCREMENT = 100;
```
```AUTO_INCREMENT``` 입력값을 바꾼다. (초기값)  
  
**추가 3**  
```
SET @@auto_increment-increment = 3;  
```
```AUTO_INCREMENT```시 증가값을 바꾼다.  

***
# 2. 데이터의 수정 : UPDATE
**구조**
```
UPDATE 테이블이름
   SET 열1 = 값1 , 열2 = 값2 ...
   WHERE 조건식 ;
```
UPDATE에서 주의해야할 점은  
WHERE절은 생략이 가능하지만 생략할 경우 테이블의 전체 행이 변경된다.  
그러니 WHERE는 특별한 경우 아니면 생략하지 말자  
  
**예시**
```
UPDATE testTbl
   SET Lname = '없음'
   WHERE FName = 'Kyichi' ;
```  

***
# 3. 데이터의 삭제: DELETE FROM
**구조**
```
DELETE FROM 테이블명 WHERE 조건식;
```
DELETE에서 주의해야할 점은  
WHERE절은 생략이 가능하지만 생략할 경우 테이블의 전체 행이 삭제된다.
즉, WHERE를 생략한다면 남은 인생도 생략될 수 있다. 
그러니 WHERE는 생략하지 말자 
  
**예시**
```
DELETE FROM testTbl
  WHERE Fname = 'Aamer' LIMIT 5;
```  
