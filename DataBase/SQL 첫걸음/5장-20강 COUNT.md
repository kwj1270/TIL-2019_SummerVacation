행 개수 구하기 - COUNT
=======================
우선 대표적인 집계함수 5개를 알아보자
```
COUNT(집합) :   행의 개수를 나타내준다
SUM(집합)   :   수치형 집합의 값들을 합친값을 반환한다. 
AVG(집합)   :   수치형 집합의 값들을 합치고 평균을 구한다.
MIN(집합)   :   수치형 집합중에서 가장 작은 값을 반환한다.
MAX(집합)   :   수치형 집합중에서 가장 큰 값을 반환한다.
```
집계함수의 특징은 복수의 값(집합)에서 하나의 값을 계산해내는 것이다.    
이렇게 집합으로부터 하나의 값을 계산하는 것을 '집계'라 부른다.     
  
# 1. COUNT로 행 개수 구하기
**구조**
```
COUNT(집합) --행의 개수를 나타내준다
```
**예시**
```
SELECT COUNT(*) FROM mytable;
```
```COUNT()```는 계산된 테이블의 행의 개수를 반환한다.    
위와 같은 코드를 입력하면 테이블의 모든 행(데이터)의 갯수를 알 수 있다.  
물론 특정 필드(열)을 입력해도 상관없다.(2번 목록에서 차이점을 확인하자)  

## 1.1. WHERE 구 지정하기 (SELECT에서 사용)  
집계함수를 SELET 구에 쓰면 WHERE구의 유무와 상관없이 결과값으로 하나의 행을 반환한다.   

**예시**
```
___________________
| id | name | age |
|____|______|____ |
|1   |'A'   | 24  |
|____|______|_____|
|2   |'A'   | NULL|
|____|______|_____|
|3   |'B'   | 23  |
|____|______|_____|
|4   |'C'   | 27  |
|____|______|_____|
|5   |NULL  | 32  |
|____|______|_____|

SELECT COUNT(*) FROM mytable WHERE name='A';

```
```
___________________
|     COUNT(*)    |
|_________________|
|        2        |
|_________________|

'A' 이름을 가진 사람은 두명이다.
```
내부적으로 SELECT 구는 WHERE 구보다 나중에 처리된다.  
따라서 WHERE 구로 조건을 지정하면 테이블 전체가 아닌, 검색된 행이 COUNT로 넘겨진다.  
  
쉽게 말해서 **WHERE 제한이 유효하게 작용된다.**   
  
***
# 2. 집계함수와 NULL 값
COUNT() 함수는 인수로 열명을 지정할 수 있다.  
그리고 그 열에 한해서 행의 개수를 구할 수 있다.  
여기서 문제점은 ```NULL```값을 취급하느냐 하는 것이다.  
본론 부터 말하면 취급 안한다.  
모든 집계함수는 기본적으로 ```NULL```을 취급 안한다. (이부분은 틀린점이 있다면 지적 부탁드립니다.)  
일단 코드를 보자  
  
**예시**
```
___________________
| id | name | age |
|____|______|____ |
|1   |'A'   | 24  |
|____|______|_____|
|2   |'A'   | NULL|
|____|______|_____|
|3   |'B'   | 23  |
|____|______|_____|
|4   |'C'   | 27  |
|____|______|_____|
|5   |NULL  | 32  |
|____|______|_____|

SELECT COUNT(id), COUNT(name) FROM mytable;

```
```
_____________________________________
|     COUNT(id)   |   COUNT(name)   |   
|_________________|_________________|
|        5        |        4        |
|_________________|_________________|

NULL 값은 제외되고 COUNT가 되었다.
```
* 결론 : 대부분의 집계함수는 집합 안에 NULL 값이 있을 경우 무시한다.

***
# 3. DISTINCT로 중복 제거
일단 집계함수가 아닌 일반적인 SELECT에서 사용하는 ```DISTINCT```를 배워보자   
 ```DISTINCT```는 값이 중복 될 경우 한개의 값만 출력해주는 키워드이다.  
 ```ALL```은 디폴트 값으로 생략될경우에도 실행 된다. ```ALL```은 중복된 값도 출력한다.  
   
**예시**
```
___________________
| id | name | age |
|____|______|____ |
|1   |'A'   | 24  |
|____|______|_____|
|2   |'A'   | NULL|
|____|______|_____|
|3   |'B'   | 23  |
|____|______|_____|
|4   |'C'   | 27  |
|____|______|_____|
|5   |NULL  | 32  |
|____|______|_____|

SELECT ALL name FROM mytable ;
 
SELECT DISTINCT name FROM mytable ;
 
```
```
ALL 결과                  DISTINCT 결과                        
________                    ________                       
| name |                    | name |                                       
|______|                    |______|                                         
|'A'   |                    | 'A'  |                                       
|______|                    |______|                                                                                            
|'A'   |                    | 'B'  |                                       
|______|                    |______|                                                 
|'B'   |                    | 'C'  |                                       
|______|                    |______|                                        
|'C'   |                    | NULL |                                       
|______|                    |______|                                      
| NULL |                                                             
|______|                                                                
```
이렇듯 ```DISTINCT```는 중복된 값을 보여주지 않는다.

***
# 4. 집계함수에서 DISTINCT
집계 함수에서도 ```DISTINCT``` 키워드를 사용할 수 잇다.   
  
**잘못된 사용법**
```
SELECT DISTINCT COUNT(*) FROM mytble;
```
이런 구조는 COUNT가 먼저 계산되기에 사용할 수 없고  
  
**올바른 사용법**
```
SELECT DISTINCT COUNT(DISTINCT *) FROM mytble;
```
집계 함수의 인수로 사용할 수 있다.   
동작은 중복을 제거한 뒤 COUNT로 개수를 구하는 것이다.  
  
**예시**
```
___________________
| id | name | age |
|____|______|____ |
|1   |'A'   | 24  |
|____|______|_____|
|2   |'A'   | NULL|
|____|______|_____|
|3   |'B'   | 23  |
|____|______|_____|
|4   |'C'   | 27  |
|____|______|_____|
|5   |NULL  | 32  |
|____|______|_____|

SELECT DISTINCT COUNT(ALL name), COUNT(DISTINCT name) FROM mytble;

```
```
______________________________________________
| COUNT(ALL name)     | COUNT(DISTINCT name) |   
|_____________________|______________________|
|         4           |          3           |
|_____________________|______________________|
```
4와 3이 나온 이유는 ```DISTINCT```도 있지만 NULL값은 포함하지 않는다.
