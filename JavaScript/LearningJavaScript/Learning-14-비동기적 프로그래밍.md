
***
# 3. 프라미스
## 3.1. 개요
```
1. 콜백함수의 값을 이용하여 코드를 실행하려 하는데 
   콜백이 실행 되었는지 안 되었는지는 명확히 알수가 없다.
   그래서 콜백의 상태에 따라 코드를 동작시키게끔 만들었다.
   
2. 콜백헬과 같이 여러 중첩된 콜백함수가 있을 때 순서를 파악하기 위하여 사용
```
프라미스는 성공하거나 실패하거나 단 두가지뿐이다.  
또한 성공이든 실패든 단 한번만 일어나기 때문에 성공한 프라미스가 나중에 실패하는 일은 없다.  
게다가 프라미스는 객체이므로 어디든 전달할 수 있다.  
### 3.1.1. 프라미스 만들기 와 상태
**프로미스 생성**
```
function countdown(seconds){
 return new Promise(function (resolve , reject){
    for(let i = seconds; i >=0; i--){
      setTimeout(function(){
        if(i>0) console.log(i + '...');
        else resolve (console.log("GO!");
      },(seconds-i)*1000)
    }
  });
});
```
**프라미스 상태**
```
pending   : 아직 약속을 수행 중인 상태

fuflilled : 약속이 지켜진 상태
            resolve()를 만난 상태
            
rejected  : 약속이 어떤 이유에서 못 지켜진 상태 
            reject()를 만난 상태
            
settled   : 약속이 일단 결론이 난 상태

```
코드를 분석하면서 설명을 하겠다.  
  
**pending**
```
 return new Promise(function (resolve , reject){
```
Promise 객체의 상태가 ```pending``` 되었다.  
```pending``` 상태는 Promise 객체가  
```resolve()```나 ```reject()```를 만나기전의 상태를 의미한다.   
  
**fulfilled**
```
 for(let i = seconds; i >=0; i--){
      setTimeout(function(){
        if(i>0) console.log(i + '...');
        else resolve (console.log("GO!");
```
```resolve()```를 만난다면 Promise 객체의 상태가 fulfilled가 된다.  
물론 이는 콜백함수 안에서 정의 되었으니 콜백함수가 끝나고 나서 상태가 변화된다.  
  
이렇듯 ```resolve()``` 와 ```reject()```는 콜백함수에 사용해야 하며  
이를 return 해주어야 Promise 객체의 상태를 변화 시킬수 있다.  
  
```resolve()```의 인자로 들어간 값은 조금 뒤에 다루겠다.
**rejected**
위에 예제에서는 ```reject()```가 없는 예제이다.  
특수한 조건을 만들어서 콜백함수 안에 ```reject()```를 만들어 사용해도 된다.  
그러면 앞서 말한 것처럼 Promise 객체의 상태가 ```rejected``` 로 바뀐다.  
    
하지만 위 예제처럼 작성하지 않아도 된다.   
왜냐하면 콜백함수가 끝나면 ```resolve()```로 인해 ```fulfilled```가 되고  
그 나머지 상태는 콜백함수가 아직 시작되지 않거나 끝나지 않았다는 뜻으로 해석되니  
이를 활용하면 되기 때문이다.
(코드의 로직에 따라 즉, 목적에 따라 다르니 꼭 그렇다는 말은 아니다.)  

그래도 이해가 안 될 수 있으니 다른 예제를 한번 보자  
  
**reject()**
```
let _promise = function (value) {
	return new Promise(function (resolve, reject) { 
		window.setTimeout(function () {
			if (value) {
				resolve("성공");
			}
			else {
				reject(Error("실패"));
			}
		}, 1000);
	});
};
```
위 예제를 보면 ```reject(Error("실패"));```를 사용했다.  
Error객체를 넘겨서 후에 에러 처리까지 할 수 있게한다.  

### 3.1.2. 프라미스 사용
Promis객체를 생성하여 countdown에 넣었다.  
그렇다면 사용방법은 달라지는 것인가? 정답은 아니다.  
기존 우리가 사용하는 대로 편하게 사용할 수 있다.
다만 추가적으로 사용할 수 있는 기능이 늘어났다고 생각하는 것이 좋다.
  
**프라미스 사용예제 1**
```
countdown(5).then(
	function(){console.log(`countdown completed successfully`);},
	function(err){console.log(`countdown experienced an error:${err.message}`);}
);
```
위 코드를 해석하기 전에 알아둘 메소드가 있다.
```
.then()	  :  Promise객체의 상태가 fulfilled일때 동작한다.
.catch()  :  Promise객체의 상태가 rejected 이거나 fulfilled이 아닐 때 동작한다.
```
즉 상태에 따라서 실행되는 메소드가 다르니 프라미스는 이를 이용하는 것이다.  
첫번째 코드를 보면 ```function(err)```가 있는데 이는 Error상황이 발생했으면 이를 실행토록 하는 코드이다.  
.```then()```안에 두가지 상태처리 메소드를 사용하는 것 대신에 ```.catch()```를 이용할 수 있다.  
  
**.catch()예제**  
```
const p = countdown(5);
p.then(
	function(){console.log(`countdown completed successfully`);},
);
p.catch(
	function(err){console.log(`countdown experienced an error:${err.message}`);}
);
```
**부록**
```
function countdown(seconds){
	return new Promise(function(resolve,reject){
		for(let i =seconds; i>=0;i--){
			setTimeout(function(){
				if(i===13) return reject(new Error("Oh my god"));
				if(i>0) console.log(i + '...');
				else resolve(console.log("GO!"));
			}, (seconds-i)*1000);
		}
	});
}

countdown(15);
```
**결과**
```
15...
14...
Uncaught (in promise) Error: Oh my god
    at <anonymous>:5:30
12...
11...
10...
9...
8...
7...
6...
5...
4...
3...
2...
1...
GO!
```
실행 결과를 보면 알 수 있듯이 함수를 멈추지는 않는다.  
resolve와 reject는 그저 프라미스의 상태를 관리 할 뿐이기 때문이다.  
앞선 예제들을 보면 두 변수를 위해 return을 사용했기에 멈추지 않는 것이다.  

### 3.1.3. 이벤트
노드에는 이벤트를 지원하는 모듈 ```EventEmitter```가 내장되어 있다.
이 모듈을 사용하여 countdown 함수를 개선해보자
```
const EventEmitter = require('events').EventEmitter;

class Countdown extends EventEmitter{
	constructor(seconds, superstitious){
		super();
		this.seconds = seconds;
		this.superstitious = !!superstitious;
	}	
	go(){
		const countdown = this;
		return new Promise(function(resolve,reject){
			for(let i =countdown.seconds; i>=0;i--){
				setTimeout(function(){
					if(countdown.superstitious &&i===13) 
						return reject(new Error("Oh my god"));
					countdown.emit('tick',i);
					if(i===0)resolve();
				}, (countdown.seconds-i)*1000);
			}
		});
	}
}			
```
EventEmitter를 상속하는 클래스는 이벤트를 발생시킬 수 있다.  
실제 countdownd을 동작하는 메소드 go()에서는 this를 가장 먼저 할당하였다.  
this를 할당해줌으로써 이 객체(인스턴스), 즉 현재 객체의 seconds를 통해 동작을 취할 수 있기 때문이다.  
여기서 this를 그냥 사용해도 되지않냐 생각할 수 있는데 this는 콜백함수 안에 들어가면 값이 바뀐다.(window)   
그래서 this의 현재값을 저장하는 변수 countdown을 사용한 것이다.  
**countdown 사용 코드**
```
const c = new Countdown(5)

c.on('tick', function(i){
	if(i > 0) console.log(i+'...');

});
c.go()
	.then(function(){
		console.log("GO!");
	})
	.catch(function(err){
		console.err("err.message");
	});
	// 체이닝 기법
```
여기서 중요한 것은 ```tick```이벤트이다.  
node.js를 배우지 않은 나는 여기를 이해하는데 꽤나 많은 시간을 투자했다.  
첫번째 예제의 ```countdown.emit('tick',i);```은
이벤트를 발생시키는 것이라 보면 되는데  
보낼때 시간을 의미하는 변수 ```i```도 같이 보낸다.    
그럼 이 이벤트는 누가 받는 것이냐?  
바로 두번째 예제의 코드인 ```c.on('tick', function(i){```가 받는 것이다.  
on()메소드는 이벤트가 일어날때 까지 기다리고 있다가 이벤트가 오면 이를 처리한다.  
즉 이벤트가 발생하면 ```if(i > 0) console.log(i+'...');```가 실행되는 것인데  
이는 콘솔창에 시간을 나타내준다  
**결과**  
```
5...
4...
3...
2...
1...
GO!
```
하지만 위 코드는 이전 코드에 이벤트를 더하기 위해서 클래스를 만든 것 뿐이지  
아직 13이 되어도 카운트다운을 멈추지 않는 문제점은 해결하지 못했다.  
이 문제를 해결하려면 더 진행할 수 없다는 사실을 아는 즉시 대기중인 타임아웃을 모두 취소하면 된다.  
**해결**
```
const EventEmitter = require('events').EventEmitter;

class Countdown extends EventEmitter{
	constructor(seconds, superstitious){
		super();
		this.seconds = seconds;
		this.superstitious = !!superstitious;
	}	
	go(){
		const countdown = this;
		const timeoutIds = [];
		return new Promise(function(resolve,reject){
			for(let i =countdown.seconds; i>=0;i--){
				timeoutIds.push(setTimeout(function(){
					if(countdown.superstitious &&i===13) 
						return reject(new Error("Oh my god"));
					countdown.emit('tick',i);
					if(i===0)resolve();
				}, (countdown.seconds-i)*1000);
			});
		});
	}
}			
```
이전 코드와 달라진 점은 아래와 같다.
```
const timeoutIds = [];
```
```
timeoutIds.push(setTimeout(function(){
```
이제 여태까지 궁금했던 코드를 해석해보자  
**해석**
```
const EventEmitter = require('events').EventEmitter;
```
node.js의 이벤트 모듈인 'events'를 통해 EventEmitter를 받았다.  
우리는 이로써 다양한 이벤트 관련 기능들을 사용할 수 있게 된것이다.
```
class Countdown extends EventEmitter{
```
기존 function이었던 countdown을 EventEmitter를 상속받기 위해 클래스화 시켰다.
```	constructor(seconds, superstitious){
		super();
		this.seconds = seconds;
		this.superstitious = !!superstitious;
	}

```
