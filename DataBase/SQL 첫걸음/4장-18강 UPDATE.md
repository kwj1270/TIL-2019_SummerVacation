데이터 갱신하기 - UPDATE
=======================
기존에 존재하는 데이터를 갱신하기 위해서는 ```UPDATE```명령어를 사용한다.  
# 1. UPDATE로 데이터 갱신하기
**구조**
```
UPDATE 테이블명 SET 열1 = 값1 WHERE 조건식 
```
UPDATE도 DELETE와 마찬가지로 ```WHERE 조건식```이 있어야한다.  
만약 정의하지 않으면 ```UPDATE 테이블명 SET 열1 = 값1 ```  
즉, 모든 행의 데이터를 각 열에 맞춰서 바꾼다.  
다시 말하면 모든 데이터의 값을 바꾼다.  
  
**예시**
```
UPDATE mytable SET name = 'Lee' WHERE id=1;  
```
id가 1인 행의 데이터의 name 은 'Lee' 로 바꾸어라

***
# 2. UPDATE로 갱신할 경우 주의사항
```SET``` 구는 ```=``` 대입 연산자를 의미한다.  
그렇다면 복합연산자처럼 자기 자신을 사용할 수 있을까? 답은 가능하다.  
  
**예시**
```
no는 1이라 가정  

UPDATE mytable SET no = no+1;
```
```
no 는 2
```
갱신은 행 단위로 처리되므로 현재의 ```no```값은 갱신하기 전 값이고  
대입 연산자를 통해 새로 받은 ```no```가 갱신 되는 것이다. 

***
# 3. 복수열 갱신
UPDATE도 물론 복수의 열(필드)을 대상으로 값을 변경 할 수 있다.  
  
**구조**
```
UPDATE 테이블명 SET 열1 = 값1, 열2 = 값2, ... WHERE 조건식 
```
**예시**
```
UPDATE mytable SET name = 'Lee', age = 25 WHERE id=1;  
```
id가 1인 행의 데이터의 name 은 'Lee' age는 25로 바꾸어라  
  
하지만 여기서 정말 중요한 점은 **각 RDBMS마다 동작이 다르다.**  

## 3.1. MySQL
**1번 예시**
```
UPDATE mytable SET no = no+1 , a = no WHERE id=1;  
```
단순히 ```no+1``` 이 계산되고 ```a```가 값을 받으니 ```no```와 ```a```의 값은 같아진다.  
  
**2번 예시**
```
UPDATE mytable SET a = no , no = no+1  WHERE id=1;  
```
이번에도 단순히 ```a```가 먼저 ```no```의 값을 받고 ```no```는 후에 ```no+1```로 값이 증가한다.  
그래서 ```a``` 와 ```no```는 ```1``` 차이가 난다.

## 3.2. Oracle
Oracle은 MySQL과 결과가 다른데 이유는 식의 순서가 처리에 영향을 주지 않기 때문이다.
**1번 예시**
```
UPDATE mytable SET no = no+1 , a = no WHERE id=1;  
```
**2번 예시**
```
UPDATE mytable SET a = no , no = no+1  WHERE id=1;  
``` 
위 코드가 동시에 동작한다고 해석하면 a는 no-1이다.    
즉 결과는 1번이든 2번이든 ```a```느 ```no```보다 ```-1```값을 가진다.  
Oracle은 이렇듯 동시에 동작한다고 가정을 하기 때문에 값이 같아질 수가 없다.     

***
# 4. NULL로 갱신하기
**예시**
```
UPDATE mytable SET name = NULL WHERE id = 1 ;
```
이를 NULL 초기화라고 한다.  
단, ```NOT NULL```로 제약이 걸린 필드(열)는 NULL 값을 넣을 수 없다.

