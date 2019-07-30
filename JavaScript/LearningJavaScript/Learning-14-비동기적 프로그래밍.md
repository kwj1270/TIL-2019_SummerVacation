비동기적 프로그래밍
=======================
> 자바스크립트의 본성 때문에 비동기적 프로그래밍이 필요하다.  

자바스크립트는 단일스레드에 동작한다.  
이는 할 수 있는 일이 제한 된다고 생각 할 수 있지만  
멀티스레드 프로그래밍이 겪어야 하는 정말 골치 아픈 문제를 신경 쓰지 않아도 된다는 장점도 있다.  
  
다만,    
**우리는 부드럽게 동작하는 소프트웨어를 만들기 위해서는 여러 문제를 비동기적 관점에서 생각해야한다.**   
  
비동기적 프로그래밍에는 3가지 페러다임이 존재한다.  
1. 콜백  
2. 프라미스  
3. 제네레이터  
  
  
**제네레이터**  
제네레이터는 제네레이터 자체로 비동기적 프로그래밍을 전혀 지원하지 않는다.  
```
제네레이터 + 특수한 콜백
제네레이터 + 프라미스
```
  
**프라미스**  
프라미스 역시 콜백에 의존한다.   
  
**콜백**  
콜백은 제네레이터나 프라미스 외에도 이벤트 처리 등에 유용하게 쓸 수 있다.  
  
**비동기적 테크닉 사용해야 하는 경우 3가지**
```
Ajax 호출을 비롯한 네트워크 요청
파일을 읽고 쓰는 등의 파일시스템 작업
의도적으로 시간 지연을 사용하는 기능(알람등) 
```
# 1. 비유
간단한 개요라고 보면 된다.  
우리가 식사를 하러 대기 손님이 많은 식당에 갔다는 가정하에  
```
콜백     :  우리의 전화번호를 받아서 자리가 나면 전화를 해준다.
            우리는 그 시간동안 다른 일들을 처리 할 수 있다.
        
프라미스 :   자리가 났을 때 진동하는 호출기를 우리에게 넘겨준다.

비유 내용이 비슷하겠지만 이는 후에 다시보면 이 뜻을 이해할 수 있을 것이다.        
```

***
# 2. 콜백(callback method)
> JavaSCript에서 가장 오래된 비동기적 메커니즘 
  
**간단한 정의** : 나중에 호출할 함수  
**사용 구역** : 다른 함수의 매개변수, 객체의 프로퍼티, 배열에 사용   
그리고 주로 익명 함수로 사용된다.
## 2.1. 예제 
### 2.1.1. setTimeout()  
setTimeout()의 기본구조는 마지막에 살펴본다.  
  
**예제 코드**
```
console.log("Before timeout: " + new Date());
function f(){
  console.log("Before timeout: " + new Date());
}
setTimeout(f , 60*1000); //1분
console.log("I happen after setTimeout!");
console.log("Me too!");
```
**결과**
```
Before timeout: Tue Jul 30 2019 20:03:32 GMT+0900 (한국 표준시)
I happen after setTimeout!
Me too!
After timeout: Tue Jul 30 2019 20:04:32 GMT+0900 (한국 표준시)

1분뒤에 callback함수 f 가 실행되니 기다리자
```
JavaScript는 싱글 스레드를 사용하기에 동기적으로 이루어진다면  
```setTimeout(f, 60*1000)```메소드에서 1분을 기다려야 다음 코드가 실행된다.(끔찍하다)  
  
결과를 통해 알 수 있는점은 callback 함수는 어떤 것도 차단하지 않는다는 점이다.  
즉 프로그램의 흐름이 부드럽게 동작하게끔 만든다.(달리 말하면 다른 작업을 할 수 있다)  
  
**비동기적 테크닉은 프로그램이 이런 식으로 멈추는 일을 막아준다**  
  
추가로 익명함수를 사용하면 
```
setTimeout(function(){
  //jS코드 여기는 js 코드 입니다.
  //jS코드 여기는 js 코드 입니다.
  //jS코드 여기는 js 코드 입니다.
  //jS코드 여기는 js 코드 입니다.
  //jS코드 여기는 js 코드 입니다.
  //jS코드 여기는 js 코드 입니다.
  //jS코드 여기는 js 코드 입니다.
} , 60*1000);
```
```,60*1000)``` 시간 매개변수를 찾기 어렵거나 익명함수의 이부분처럼 보이니 주의하자.  
자연 매개변수는 마지막 행에 쓴다는 원칙을 세운다면 이런 혼란은 피할 수 있다.
### 2.1.2. setInerval 과 clearInterval
> setTimeout은 1회만 실행되지만  
> setInterval은 정해진 주기마다 콜백함수를 실행한다.  
> setInterval을 끝내려면 (보통 조건절에)clearInterval을 사용한다.  
   
**예제코드**
```
const start = new Date();
let i = 0;
const IntervalId = setInterval(function(){
  let now = new Date();
  if(now.getMinutes() !== start.getMinutes() || ++i>10)
    return clearInterval(IntervalId);
  console.log(`${i}: ${now}`);  
}, 5*1000)
```
실제로 시간이 걸리기에 필자도 정리를 위해 현재 문서를 작성하고 있다.
**결과**
```
Tue Jul 30 2019 20:32:43 GMT+0900 (한국 표준시)
Tue Jul 30 2019 20:32:48 GMT+0900 (한국 표준시)
Tue Jul 30 2019 20:32:53 GMT+0900 (한국 표준시)
Tue Jul 30 2019 20:32:58 GMT+0900 (한국 표준시)
```
코드에 대한 간단한 해석은
현재 분 과 함수가 실행될때의 분이 다르거나 또는 i가 10을 초과하면 멈추는 것이다.
**요점**
```
const IntervalId = setInterval(function(){
```
위 코드를 보면 setInteval이 ID를 반환한다는 사실을 알 수 있다.
이 ID를 써서 ```clearInterval(ID명)```을 사용하여 ```setInterval```을 멈출 수 있다.
### 2.1.3 스코프와 비동기적 실행
비동기적 실행에서 혼란스럽고 에러도 자주 일어나는 부분은   
**스코프**와 **클로저**가 비동기적 실행에 영향을 미치는 부분이다.  
  
**예제코드(실패)**
```
function countdown(){
  let i;
  console.log("Countdown:");
  for(i = 5 ; i >= 0 ; i-- ){
    setTimeout(function(){
      console.log(i === 0 ? "GO" : i);
    }, (5-i)*1000);
  }
}
```
위의 코드는 let i 는 블록스코프이지만 그 스코프가 함수이기 때문에  
for()가 끝나도 i는 존재하는 것이다. 그러므로 콜백함수가 적용이 되어서   
```
-1
-1
-1
-1
-1
```
위와 같은 결과가 나타난다.  
그렇다면 어떻게 해결해야 하는가?  
  
**예제코드(성공)**
```
function countdown(){
  console.log("Countdown:");
  for(let i = 5 ; i >= 0 ; i-- ){
    setTimeout(function(){
      console.log(i === 0 ? "GO" : i);
    }, (5-i)*1000);
  }
}
```
let i를 for()의 넣어 주었다.  
let의 블록스코프라는 특성상 let은 for(){}안에만 존재하게 되고  
이는 콜백함수가 for가 끝난 후 i를 사용할 수 없으니 for(){} 안에서 즉시 실행이 된다.
```
5
4
3
2
1
GO
```
  
콜백은 자신을 선언한 스코프(클로저)에 있는 것에 접근 할 수 있다.
이러한 원칙은 모든 비동기적 테크닉에 적용이되니 주의하여 사용하자.

### 2.1.4 오류 우선 콜백
콜백을 사용하면 예외처리가 어려워진다.(동작을 나중에 하므로)  
그래서 콜백과 관련된 표준이 필요해졌고  
이에 따라 콜백의 **첫번째 매개변수**에 **에러 객체**를 쓰게 되었다.

```
const fs = require('fs');

const frame= 'may_or_may_not_exist.txt';
fs.readFile(fname, function(err, data){
  if(err) return console.error(`error reading file ${fname} : $ {err.message}`);
  console.log(`${name} contents: ${data}`);
});
```
콜백에서 가장 먼저 하는 일은 err이 참 같은 값인지 확인하는 것이지만  
가장 중요한 일은 return 으로 빠져 나오는 것이다.  
위의 코드는 err 발생시 빠져 나오게 되어있다.  
이유는 콜백 함수를 사용할 때 대부분의 사람들은 성공을 한다는 기준으로 작성하기에  
콜백함수가 실패한다면에 대한 예외처리가 없는 것이다.  

### 2.1.5 콜백 헬
콜백을 사용해 비동기적으로 실행할 수 있지만, 현실적인 단점이 있다.  
한번에 여러가지를 기다려야 한다면 콜백을 관리하기가 상당히 어려워진다.
```
const fs = require('fs');

fs.readFile('a.txt' , function(err, dataA){
  if(err) console.error(err);
  fs.readFile('b.txt' , function(err, dataB){
    if(err) console.error(err);
    fs.readFile('c.txt' , function(err, dataC){
      if(err) console.error(err);
      setTimeout(function(){
        fs.writeFile('d.txt', dataA+dataB+dataC, function(err){
          if(err)console.error(err);
        });
       } , 60*1000);
    });
  });  
});
```
프로그래머들은 이러한 코드를 콜백 헬이라 부른다.(중괄호로 둘러싸여 끝없이 중첩된 콜백 코드)
일단 위 예제는 에러를 기록한다.
그러나 더욱 골치 아픈 문제는 **예외**를 일으키는 것이다.

```
const fs = require('fs');
function readSketchFile(){
  try{
    fs.readFile('does_not_exist.txt',function(err,data){
      if(err) throw err;
    });
  } catch(err){
      console.log("warning: minor issue occurred, program continuing");
  }
}
readSketchy();
```
위 예제는 얼핏 문제가 없는 것 같지만  
사실 **실행 되지 않는다.**  
그 이유는 try...catch 블록은 같은 함수 안에서만 동작하기 때문이다.    
   
readSketchyFile 함수 안에 있지만,    
정작 예외는 콜백으로 호출하는 **익명함수** 안에서 일어났다.  
  
JavaScript에서는 콜백이 우연히 두번 호출되거나, 아에 호출되지 않는 경우를방지하는 안정 장치가 없다.  
비동기적 코드가 늘어나면 늘어날 수록 버그가 없고 관리하기 쉬운 코드를 작성하는 것은 매우 어려우니  
**프라미스**가 등장하였다.

***
# 3. 대주제
> 인용
## 3.1. 소 주제
### 3.1.1. 내용1
```
내용1
```
