
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
			}
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
```
constructor(seconds, superstitious){
		super();
		this.seconds = seconds;
		this.superstitious = !!superstitious;
	}
```
클래스화가 되었으니 생성자를 사용한다.  
일단 클래스의 기본 문법인 상위클래스의 생성자를 호출하고,  
```seconds```라는 시간을 입력받아서 사용자 임의로 시간을 정할 수 있고  
```superstious``` boolean 타입을 받아서 13은 불길한 숫자이니   
믿으면 멈추고 안믿으면 안 멈추고를 정할 수 있다. 우리는 멈춰야 하니 true 값을 준다.  
```
go(){
		const countdown = this;
		const timeoutIds = [];	
```
앞서 말했듯이 ```this```를 콜백함수 안에서 사용해야 하는데 순수 ```this```를 사용하면 특성에 의해 값이 변한다.  
그래서 이를 대신하는 countdown이라는 변수를 생성하고  
이전에 없던 timeoutIds 배열(객체)를 만든다. 이는 차후에 설명하겠다.
```
return new Promise(function(resolve,reject){
```
Promise 객체를 만들고
```
for(let i =countdown.seconds; i>=0;i--){
				timeoutIds.push(setTimeout(function(){
					if(countdown.superstitious &&i===13) {
						timeoutIds.forEach(clearTimeout);
						return reject(new Error("Oh my god"));
					}
					countdown.emit('tick',i);
					if(i===0)resolve();
				}, (countdown.seconds-i)*1000);
			}
```
for 구문을 돌린다. 여기서  
이전에는 일반적인 방식으로 콜백 함수인 setTimeout()을 사용했지만  
지금은 배열 timeoutIds 에 차곡차곡 넣고 있다.  
이유는 간단하다 배열에 각 setTimeout()을 저장시켜 순서를 만들었는데,  
이를 forEach를 통해서 13초일 경우 기존 저장된 내용물을 삭제하는 식이다.  
아직 초보자인 나는 이 코드를 보고 정말 놀라웠다.    
그래서 결론은 13초가 되면 대기중인 타임아웃을 모두 취소시켜서 코드를 완성하였다.  

### 3.1.4. 프라미스 체인
Promise에는 체인으로 연결할 수 있다는 장점이 있다.  
즉, Promise가 완료되면 다른 프라미스를 반환하는 함수를 즉시 호출할 수 있다.   
**예제**
```
function launch(){
	return new Promise(function(resolve,reject){
		console.log("Lift off!");
		setTimeout(function(){
			resolve("In orbit!");
		}, 2*1000);
	});
}
```
**실행**
```
const c = new Countdown(5)
		.on('tick', i => console.log(i + '...'));
c.go()
		.then(launch)
		.then(function(msg){
			console.log(msg);		// "In orbit!"
		})
		.catch(function(err){
			console.log("Houston, we have a problem....");
		})		
```
**해석**
처음에는 ```go()``` 메소드를 통해서 Promise 객체를 받는다.  
콜백이 성공 하였으면 ```launch()```를 실행하는데  
```launch```또한 Promise 객체를 이용하니 
```.then(function(msg){```처럼 이를 체이닝으로 또 연결 가능하다.
```msg```는 ```launch()```의 ```resolve("In orbit!");```의 In orbit!을 나타내므로 이를 출력한다.  
```.catch(function(err){``` 같은 경우는 이렇게 사용되는중 어디서든 에러가 발생하던가 아니면 콜백이 실행되지 않았을 경우  
실행 되는데 이것도 마찬가지로 체이닝 기법을 사용할 수 있다. 사실 이게 핵심이다. 
에러가 생기면 체인 전체가 멈추고 .catch() 체인이 실행되기에  
만약 15초를 넣고 돌리면 launch는 실행이 되지 않을 것이다.

### 3.1.5. 결정되지 않은 프라미스 방지하기
Promise를 사용할 때 우리는 ```resolve``` 와 ```reject```를 통해  
코드를 단순화 하고 콜백이 두번 이상 실행되는 문제를 방지한다.  
하지만 정작 ```resolve``` 와 ```reject```를 사용하는 것을 까먹는 경우 이를 해결하기는 쉽지않다.  
그래서 이를 예방하는 방법이 있는데 바로 ```setTimeout()```을 거는 것이다.
기존에 ```setTimeout()```을 많이 사용했기에 헷갈릴 수 있겠지만  
이전에는 countdown을 위한 콜백함수 였다면  
지금은 시간이 지나면 resolve로 예약을하는 ```setTimeout()```이라 생각하길 바란다.  
**예제 1**
```
function launch(){
	return new Promise(function(resolve, reject){
		if(Math.random() < 0.5) return;		// 아... 여기서 정의를 안했다.
		console.log("Lift off!");
		setTimeout(function(){
			resolve("In orbit!");
		}, 2*1000);
	});
}
```
이 lauch는 reject를 호출하지 않는데다가,  
심지어 콘솔에 기록하지도 않았다.  
즉, 10번 시도중 5번은 영문도 모른채 실패하는 셈이다.  
이제 이코드를 담을 ```addTimeout()```메소드를 작성해보자
**예제 2**
```
function addTimeout(fn,timeout){
	if(timeout === undefined) timeout = 1000;
	return function(...args){
		return new Promise(function(resolve,reject){
			const tid = setTimeout(reject,timeout,newError("promise TImed out"));
			fn(...args)	
				.then(function(...args){
					clearTimeout(tid);
					resolve(...args);
				})
				.catch(function(...args){
					clearTimeout(tid);
					reject(...args);
				})
		})

	}
}
```
이 코드는 해석하기 정말 까다롭다...... 나중에 다시 봐야겠다.
**사용**
```
c.go()
	.then(addTimeout(launch, 11*1000))
	.then(function(msg){
		console.log(msg);
	})
	.catch(function(err){
		console.log("Houston, we have a problem: " + err.message);
	})
```
사용하는 코드는 예제코드에 비하면 아주 쉽다.
이제 launh 함수에 문제가 있더라도 프라미스 체인은 항상 결정된다.
