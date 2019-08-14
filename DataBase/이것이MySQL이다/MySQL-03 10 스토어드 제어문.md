# 1. IF..ELSE   
스토어드 프로시저는 프로그래밍 기능이기에    
C언어 JAVA와 같은 응용프로그래밍 언어에서 사용하는 조건문,반복문 구조를 비슷하게 구현해 놓았다.    
   
**구조**
```
IF <조건식 BOOLEAN> THEN  
       SQL문장식들 1
ELSE IF <조건식 BOOLEAN> THEN 
       SQL문장식들 2
ELSE
       SQL문장식들 3
END IF;       
```

**예시**
```
DROP PROCEDEURE IF EXISTS ifProc;
DELIMITER $$
CREATE PROCEDURE ifProc()
BEGIN
  DECLARE var1 INT;
  SET var1 = 100;

  IF <var1 > 100> THEN  
       SELECT '100 초과 입니다.';
  ELSE IF <var1 == 100> THEN 
       SELECT '100 입니다.';
  ELSE
       SELECT '100 미만 입니다.';
  END IF;  
END $$
DELIMITER ;
CALL ifProc();
```
응용프로그래밍 언어에서의 ```IF...ELSE``` 와 다른점은  
```IF```와 ```ELSE IF```를 사용할 경우 **THEN**을 붙여야 된다.  
반대로 ELSE는 붙이지 않아도 된다.  
그리고 마지막에는 ```END IF;``` 구문으로 끝을 나타내주어야 한다.

***
# 2. CASE
```IF...ELSE``` 구문은 '2중 분기'라는 용어를 종종 사용한다.  
즉, 참 아니면 거짓 두 가지만 있기 때문이다.  
```CASE```는 ```IF...ELSE```와 다르게 **'다중 분기'** 를 사용하는 제어문이다.    
   
**구조**
```
CASE [조건 대상]
  WHEN [조건식] THEN
       SQL문장식들 1
  WHEN [조건식] THEN
       SQL문장식들 2
  WHEN [조건식] THEN
       SQL문장식들 3       
  WHEN [조건식] THEN
       SQL문장식들 4
  ELSE
       SQL문장식들 5
END CASE;
```
```IF```와 마찬가지로 **THEN**을 사용하고 ```ELSE```에서는 사용하지 않는다.  
```CASE```에 ```[조건 대상]```이 있는데 간단히 설명하자면     
```조건 대상을```을 설정시 ```WHEN```에서 조건 대상을 생략할 수 있다.    
그러나 조건 대상을 설정하지 않는 것이 조금더 자유로운 코드를 작성할 수 있다.  
  
**예시**
```
DROP PROCEDEURE IF EXISTS caseProc;
DELIMITER $$
CREATE PROCEDURE caseProc()
BEGIN
  DECLARE point INT;
  DECLARE credit CHAR(1);
  SET point = 77;
  
  CASE 
    WHEN point >= 90 THEN
         SET credit = 'A';
    WHEN point >= 80 THEN
         SET credit = 'B';
    WHEN point >= 70 THEN
         SET credit = 'C';
    WHEN point >= 60 THEN
         SET credit = 'D';
    ELSE
         SET credit = 'F';
  END CASE;
  SELECT CONCAT('취득 점수 ==>', point), CONCAT('학점 ==>', credit);
END $$
DELIMITER ;
CALL caseProc();
```  
```CASE 문```은 조건식이 TRUE 이면 해당 영역에 기술된 SQL문이 실행된다.    
만약 조건에 맞는 ```WHEN```이 여러 개이더라도 먼저 조건이 만족하는 ```WHEN```이 처리된다.  
```CASE 문```은 주로 ```SELECT```에서 사용하기도 한다. (스토어드 프로시저가 아닌 그 상태에서 기술함)    
  
***
# 3. WHILE 과 ITERATE/LEAVE
응용 프로그래밍의 반복문 ```WHILE```과 동일하다.    
다른 문들과 마찬가지로 사용법이 조금 다르다.    
   
또한,  
```ITERATOR``` 는 ```continue```   
```LEAVE``` 는 ```break```를 의미한다.   
  
**구조**
```
WHILE <조건식 BOOLEAN> DO
      SQL 명령문들...
END WHILE;

그리고 

WHILE <조건식 BOOLEAN> DO
      IF <조건식 BOOLEAN> THEN
         ITERATE;
      ELSE IF <조건식 BOOLEAN> THEN
         LEAVE;
      ELSE
         SQL 명령문들...         
END WHILE;
```
```WHILE```에서는 **DO**를 사용해주어야 한다.(그렇다고 DO WHILE은 아니다.)     
두번째 구조는 단순한 예를 든거지 그 안에 원하는 SQL 명령어들을 넣어도 된다.   
  
**예시**
```
DROP PROCEDURE IF EXISTS whileProc();
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
  DECLARE i INT;
  DECLARE hap INT;
  SET i = 1;
  SET hap = 0;
  
  WHILE (i <= 100) DO
    SET hap = hap + i;
    SET i = i + 1;
  END WHILE;  
  
  SELECT hap;
END $$
DELIMITER ;
CALL whileProc();
```
```
DROP PROCEDURE IF EXISTS whileProc2();
DELIMITER $$
CREATE PROCEDURE whileProc2()
BEGIN
  DECLARE i INT;
  DECLARE hap INT;
  SET i = 1;
  SET hap = 0;
  
  myWhile: WHILE (i <= 100) DO
    IF(i%7 = 0) THEN
      SET i = i + 1;
      ITERATE myWhile;
    END IF;
    
    SET hap = hap + i;
    IF (hap > 1000) THEN
      LEAE myWhile;
    END IF;
    SET i = i + 1;
 END WHILE;  
 SELECT hap;
END $$
DELIMITER ;
CALL whileProc2();
```

***
# 4. 오류 처리
MySQL은 오류가 발생할 경우 직접 오류를 처리하는 방법을 제공한다.
  
**구조**  
```
DECLARE 액션 HANDLER FOR 오류조건 처리할_문장;
```
**액션**
```
오류 발생 시에 행동을 정의하는데 CONTINUE와 EXIT 둘 중 하나를 사용한다.
CONTINUE 가 나오면 제일 뒤의 '처리할_문장' 부분이 처리된다.
```
**오류조건**
```
어떤 오류를 처리할 것인지를 지정한다. 
여기에는 MySQL의 오류 코드 숫자가 오거나 
SQLSTATE '상태코드', SQLEXCEPTION, SQLWARNING, NOT FOUND 등이 올 수 있다.
SQLSTATE에서 상태코드는 5자리 문자열로 되어 있다.
SQLEXCEPTION 은 대부분의 오류를,
SQL WARNING 은 경고메시지를,
NOT FOUND는 커서나 SELECT...INTO 에서 발생되는 오류를 의미한다.
```
**처리할 문장**
```
처리할 문장이 하나라면 한 문장이 나오면 되며, 처리할 문장이 여러 개일 경우에는 BEGIN...END로 묶어줄 수 있다.
```
  
**예시 1**
```
DROP PROCEDURE IF EXISTS errorProc;
DELIMITER $$
CREATE PROCEDURE errorProc()
BEGIN
     DECLARE CONTINUE HANDLER FOR 1146 SELECT '테이블이 없어요ㅠㅠ' AS '메시지';  --1146 대신에 SQLSTATE '42S02'도 가능
     SELECT * FROM noTable;
END $$
DELIMITER ;
CALL errorProc();
```
위 예제는 테이블이 없을 경우에 오류를 직접 처리하는 코드이다.  
```DECLARE``` 부분이 있어서 사용자가 지정한 메시지가 출력된다.    
만약 저부분이 없으면 MySQL이 직접 오류 메시지를 발생시킨다.    
  
**예시 2**
```
DROP PROCEDURE IF EXISTS errorProc2;
DELIMITER $$
CREATE PROCEDURE errorProc2()
BEGIN
     DECLARE CONTINUE HANDLER FOR SQLEXCEPTION,
     BEGIN
          SHOW ERRORS;
          SELECT '오류가 발생했네요. 작업은 취소시켰습니다.' AS '메시지';
          ROLLBACK;
     END;
     INSERT INTO userTbl VALUES('LSG', '이상구' , 1988, '서울', NULL,
          NULL, 170, CURRENT_DATE());
END $$
DELIMITER ;
CALL errorProc2();
```
위 예제는 기본키에 이미 저장된 값을 한번더 입력시 오류를 발생시키는 코드이다.  
  
**SHOW ERRORS** : 오류에 대한 코드와 메시지를 출력    
**ROLLBACK** : 진행중인 작업을 취소시며 COMMIT은 작업을 완전히 확정시키는 구문이다.       
    
또한 ```SHOW COUNT(*)ERRORS 문```은 발생된 오류의 개수를 출력해주며,  
```SHOW WARNINS 문```은 경고에 대한 코드와 메시지를 출력한다.   
