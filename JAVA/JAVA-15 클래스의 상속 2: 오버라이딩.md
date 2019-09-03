클래스의 상속 2: 오버라이딩.md
=======================
# 1. 상속의 기본 조건 'IS-A 관계'
```
하위 클래스는 상위 클래스의 모든 특성을 지닌다.
거기에 더하여 하위 클래스는 자신만의 추가적인 특성을 더하게 된다.
```
이러한 '상속'의 특성을 현실 세계에서도 찾아 볼 수 있다.  
예를 들면 ```핸드폰```과 ```스마트폰```이다.
```
모든 스마트폰은 핸드폰이다. (스마트폰 IS A 핸드폰)  // O 성립
모든 핸드폰은 스마트 폰이다. (핸드폰 IS A 스마트폰) // X 비성립
_______________________________________________________________
이 외에도 

노트북은 컴퓨터이다.  
전기 자동차는 자동차이다.
```
이외에도 이를 ```스마트폰은 모바일 폰을 상속받는다.```로 해석이 가능하고 이는 상속의 관계성을 보여준다.  
   
**결론**
   
* IS-A 관계는 'O은 OO이다'로 표현되는 관계이다.    
* 상속이 갖는 문법적 틍성은 'IS-A 관계'의 표현에 적합하다.   
* 따라서 상속 관계를 형성하기 위한 두 클래스는 IS-A 관계에 있어야 한다.  
  
**예시 코드**
```
class MobilePhone{
	protected String number;
	
	public MobilePhone(String number) {
		this.number = number;
	}
	
	public void answer() {
		System.out.println("HI ~ from " + number);
	}
}

class SmartPhone extends MobilePhone{
	private String androidVer;
	
	public SmartPhone(String num, String ver) {
		super(num);
		this.androidVer = ver;
		// TODO Auto-generated constructor stub
	}
	public void playApp() {
		System.out.println("App is running in " + androidVer);
	}
}

public class MobileSmartPhone {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		SmartPhone phone = new SmartPhone("010-1234-5678", "Nought");
		phone.answer();
		phone.playApp();
	}
}
```
스마트폰은 모바일폰이 갖는 기능을 모두 갖는다.    
모바일 폰은 상위 클래스로, 스마트폰은 이를 상속하는 하위 클래스로 설계하는 것이 이치에 맞는다.     
     
***
# 2. 메소드 오버라이딩
## 2.1. 상위 클래스의 참조변수가 참조할 수 있는 대상의 범위(다형성)    
상위 클래스 참조변수는 하위 클래스의 인스턴스를 참조할 수 있다.        
```
SmartPhone phone = new SmartPhone("010-555-777", "Nougat");
MobilePhone phone = new SmartPhone("010-123-456", "Oreo");
```
이렇게 상위 클래스 참조변수가 하위 클래스의 인스턴스를 참조하게 될 경우 몇가지 제한사항이 생긴다.       
바로 **참조변수의 형을 기준으로 접근 가능한 멤버를 제한한다.**         
즉, 상위 클래스의 변수, 메소드를 사용할 수 있지만 하위 클래스의 변수, 메소드는 사용할 수 없다.        
(후에 오버라이딩을 배우는데 이로 인해 하위 클래스의 메소드를 사용할 수도 있다.)       
    
상위 클래스 상속 구조상 하위에 있는 클래스들을 참조할 수 있다.        
즉, 하위 클래스의 하위 클래스도 참조할 수 있고 그 밑으로도 참조가 가능하다.    
   
## 2.2. 참조변수 간 대입과 형 변환 
```
CheeseCake ca1 = new CheeseCake();
Cake ca2 = ca1; //가능!
```
```ca1 참조변수```는 ```CheeseCake 형 참조변수```이기 때문에 ```Cake ca2```로 참조가 가능하다.  
```
Cake ca1 = new CheeseCake();
CheeseCake ca2 = ca1; // 에러 발생
```
```ca1 참조변수```는 ```Cake 형 참조변수```이기 때문에 ```CheeseCake 인스턴스```를 참조하더라도    
```CheeseCake ca2```로 참조가 불가능하다. (애초에 사용 가능 범위도 ```Cake```이기 때문이다.)         
이유는 ```Cake```를 참조하는지 ```CheeseCake```를 참조하는지 확실히 보장하지 않기 때문이다.     
    
## 2.3. 클래스의 상속과 참조변수의 참조 가능성: 배열 관점에서 정리
앞서 상위 클래스형 참조변수가 하위 인스턴스를 참조할 수 있다는 것을 배웠다.
```
Cake cake = new CheeseCake();
```
이러한 관계는 배열에서도 이어진다.  
```
CheeseCake[] cake = new CheeseCake[10];
Cake[] cakes = new CheeseCake[10];
```
    
## 2.4. 메소드 오버라이딩
상위 클래스에 정의된 메소드를 하위 클래스에서 다시 정의하는 행위를 가리켜 '메소드 오버라이딩'이라 한다.     
여기서 오버라이딩은  '무효화 시키다.'의 뜻으로 해석이 된다.  
   
오버라이딩이 성립된 경우 상위 클래스의 해당하는 메소드의 정의는 무시되며  
하위 클래스에서 정의된 메소드로 동작을 하게 된다.   
  
물론 무조건 오버라이딩이 되는 것이 아니며 하위 클래스를 사용 했을 경우에만 오버라이딩 된다.      
즉, ```상위 클래스 참조변수 = 상위 클래스 인스턴스```는 상위 클래스의 메소드를 사용하는 것이고    
```상위 클래스 참조변수 = 하위 클래스 인스턴스``` 이거나         
```하위 클래스 참조변수 = 하위 클래스 인스턴스``` 일 경우 오버라이딩 처리가 진행된다.  

**오버라이딩 조건**      
      
* 메소드 이름   
* 메소드의 반환형   
* 메소드의 매개변수 선언    
     
위 3가지 조건이 상위 클래스의 메소드와 동일할 경우 '오버라이딩'이 성립한다.       
```
class Cake{
  public void yummy(){
    System.out.println("Yummy Cake");
  }
}

class CheeseCake extends Cake{
  public void yummy(){ 			    // Cake의 yummy 메소드를 오버라이딩 함
    System.out.println("Yummy CheeseCake"); 
  }	
}

class YummyCakeOverriding{
  public static void main(String[] args){
    Cake c1 = new CheeseCake();
    CheeseCake c2 = new CheeseCake();
    Cake c3 = new Cake();
    
    c1.yummy();		// 오버라이딩된 Yummy() 호출
    c2.yummy();		// 오버라이딩된 Yummy() 호출
    c3.yummy();		// Cake의 Yummy() 호출
  }
}
```
앞서 다형성으로 상위 클래스형 참조변수로 하위 인스턴스를 참조했는데   
만약 하위 인스턴스에 오버라이딩 하는 메소드가 존재한다면     
상위 클래스형 참조변수는 상위 클래스의 멤버만 사용할 수 밖에 없지만     
메소드는 오버라이딩 된 상태로 사용한다.        
       
그리고 이러한 동작은 상위 클래스의 하위에 있는 모든 인스턴스에 대해서 동일하게 적용된다.         
즉, ```상위 클래스형 참조변수 = 하위 클래스의 하위 클래스의 인스턴스``` 일 경우     
'메소드 오버라이딩' 조건이 성립되면 ```하위 클래스의 하위 클래스의 인스턴스```의 메소드를 사용하게 된다.      
       
만약 하위 클래스에서 오버라이딩 되지 않은 상위 클래스의 메소드를 사용하고 한다면 ```super```를 이용하면 된다.       
```
public void yummy(){
  super.yummy();
  System.out.println("Yummy CheeseCake");
}
```
  
## 2.5. 인스턴스 변수와 클래스 변수도 오버라이딩의 대상이 되는가?     
결론부터 말하자면 변수는 오버라이딩 되지 않는다.(클래스 변수도 마찬가지이다.)  
변수는 메소드와 달리 클래스의 영역이 다르면 오버라이딩 하지 않고 별개의 존재로 인식한다.   
그렇기에 하위 클래스에서 상위클래스에 존재하는 동일한 이름의 변수를 ```super```를 붙여서 별개로 취급한다.  
```
class A{
  int size = 1;
  public void showVal(){
    System.out.println(size); // 1 출력
  }
}

class AA extends A {
  int size = 10;

  public void showVal(){
    System.out.println(super.size); // 1 출력
    System.out.println(size); // 10 출력
  }
}
```
무엇보다도 ```상위 클래스형 참조변수 = 하위 인스턴스``` 구조에서 변수는 오버라이딩이 되지 않기에    
메소드는 오버라이딩 되더라도 변수는 상위 클래스의 변수를 사용하게 된다.   
  
***
# 3. instanceof 연산자
## 3.1. instanceof 연산자의 기본
```instanceof 연산자```는  
참조변수가 참조하는 인스턴스의 '클래스'나 참조하는 인스턴스가 '상속하는 클래스'를 묻는 연산자이다.  
반환 값은 ```true```또는 ```false``` 이고 주로 조건문에서 사용된다.    
```
참조하는 인스턴스 instanceof 클래스
```
이것만 보기에는 이해를 하기가 힘들다. 
그래서 밑에 예시를 한번 보도록하자  
```
Cake cake = new CheeseCake();

if(cake instanceof Cake){
  ..... 생략 .....
}
```
```cake 변수```가 참조하는 인스턴스(클래스)는 ```Cake 클래스```를 상속하냐고 묻는 것이다.  
하지만 이보다 더욱 쉽게 이해하는 방법이 있다.  
바로 ```cake 변수```가 참조하는 인스턴스를 ```Cake 클래스 형```이 참조 할 수 있어?
여기서 ```cake 변수```는 ```CheeseCake 인스턴스```를 참조하고 있기에  
```Cake 클래스 형```이 ```CheeseCake 인스턴스```를 참조 할 수 있어? 라고 묻는 것이고  
이는 당연히 상위 클래스형 참조 변수는 하위 인스턴스를 참조할 수 있으니 반환 값은 ```true``` 이다.
```
a instanceof B 

a 가 참조하는 인스턴스 B가 참조 가능해?
```
이로인해 다시 한번 더 ```instanceof 연산자```를 정의해본다면    
연산자 ```instanceof```는 **명시적 형 변환의 가능성을 판단해주는 연산자이다.**
