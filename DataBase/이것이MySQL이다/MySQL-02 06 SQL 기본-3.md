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
또한 LIMIT과 도 같이 쓰일 수 있다    
  
# 3.1. 기타 삭제 구문들과 함께 보기  
```
DELETE FROM bigTbl1;                DML
DROP TABLE bigTbl2;                 DDL
TURNCATE TABLE bigTbl3;             DDL
```
DELETE는 DML로 트랜잭션 로그를 기록하는 작업 때문에 삭제하는데 오래 걸린다.      
반면에 DROP 과 TURNCATE는 DDL로 트랜잭션을 사용하지 않기에 시간소모가 적다.      
      
결론은 테이블 트랜잭션의 장점을 이용하지 않는다는 가정하에      
테이블 자체를 삭제하고 싶으면 ```DROP```을 사용하고      
테이블 구조를 남기고 데이터를 삭제하고 싶으면 ```TURNCATE```를 사용하자      
    
***
# 4. 조건부 데이터 입력, 변경
## 4.1. IGNORE
**예시**
```
기존 memberTBL에 'BBK'가 있다고 가정하에

INSERT INTO memberTBL VALUES('BBK','비비코','미국');    ->    입력 X 여기서 멈춤
INSERT INTO memberTBL VALUES('SJH','서장훈','서울');    ->    동작 X
INSERT INTO memberTBL VALUES('BBK','현주엽','경기');    ->    동작 X

SELECT * FROM memberTBL;      ->       확인
```

INSERT시에 기본 키에 중복된 데이터를 입력하면 어떻게 될까? 당연히 동작을 안한다.      
하지만 우리는 수많은 기본 키 중에서 무엇이 중복 되었는지, 안 되었는지 확인할 수 없는 경우도 있다.      
그러면 다수의 데이터를 입력할때 중복된 키로 인해서 나머지 중복되지 않은 키들도 동작하지 말아야 되는가? 그건 아니다    
그래서 SQL에서는 ```IGNORE```라는 키워드를 제공해준다.       
     
**예시**
```
기존 memberTBL에 'BBK'가 있다고 가정하에

INSERT IGNORE INTO memberTBL VALUES('BBK','비비코','미국');    ->    입력 X
INSERT IGNORE INTO memberTBL VALUES('SJH','서장훈','서울');    ->    동작 O
INSERT IGNORE INTO memberTBL VALUES('BBK','현주엽','경기');    ->    동작 O

SELECT * FROM memberTBL;      ->       확인
```
이제 PK가 중복되더라도 오류를 발생시키지 않고 무사히 넘어간다.     
  
## 4.2. ON DUPLCATE KEY UPDATE  
이번에는 중복시 넘어가는 것이 아니라 새로운 값으로 덮어쓰기를 해본다.    
    
**예시**
```
기존 memberTBL에 'BBK'가 있다고 가정하에

INSERT INTO memberTBL VALUES('BBK','비비코','미국')
  ON DUPLCATE KEY UPDATE name= '비비코', addr='미국';

INSERT INTO memberTBL VALUES('DJM','동짜몽','일본')
  ON DUPLCATE KEY UPDATE name= '동짜몽', addr='일본';

SELECT * FROM memberTBL;      ->       확인
```  
ON DUPLCATE KEY UPDATE 는     
기본 키가 중복되지 않으면 그대로 사용을 하고      
기본 키가 중복이라면 값을 UPDATE 하겠다는 의미로    
기존 값을 새로운 값으로 수정한다. (일종의 덮어쓰기라고 본다.)   
