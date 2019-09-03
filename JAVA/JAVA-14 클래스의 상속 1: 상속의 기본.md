클래스의 상속 1: 상속의 기본.md
=======================
# 1. 상속의 기본 문법 이해
## 1.1. 상속에 대한 매우 치명적인 오해
상속의 이유와 목적을 물어보면 다음과 같이 답을 하는 경우를 매우 흔하게 본다.
```
상속은 코드의 재활용을 위한 문법입니다.  
```
     
그러나 객체지향을 이핵하고 상속에대한 관점을 깊게 살펴보면 아래와 같이 답할 수 있을 것이다.    
```
연관된 일련의 클래스들에 대해 공통적인 규약을 정의할 수 있다.  
```
이러한 이유는 차근차근 배워가면서 익히도록 하자. 
  
## 1.2. 상속의 가장 기본적인 특성
상속을 단순하게 설명하면,  
기존에 정의된 클래스에 메소드와 변수를 추가하여 새로운 클래스를 정의하는 것이 상속이다.     
```
class Man{
  String name;
  public void tellYourName(){
    System.out.println("My name is " + name);
  }
}

class BusinessMan extends Man{
  String company;
  String position;
  public void tellYourInfo(){
    System.out.println("My company is " + company);
    System.out.println("My position is " + position);
    tellYourName();
  }
}
```
```
BusinessMan man = new BusinessMan();
```
위와 같이 ```Man 클래스```를 정의하고 ```BusinessMan 클래스```에서 ```Man 클래스```를 상속 받게끔 정의하였고        
```BusinessMan 클래스```에 대한 인스턴스를 생성하여 이를 ```man 참조 변수```로 참조를 시켰다.          
즉 ```man -> BusinessMan 인스턴스``` 인 형태이다.       
     
이때 ```man```이 참조하는 ```BusinessMan 인스턴스```의 아래와 같은 형태를 가진다.    
```
         _____________________________
man ->  |String name                  |
        |String company               |
        |String position              |
        |void tellYoutName            |
        |void tellYoutInfo            |
        |_____________________________|
```
위 형태의 결과로 알 수 있듯이    
```BusinessMan 인스턴스```에는 ```Man 클래스```의 변수와 메소드가 존재한다.    
이는 ```BusinessMan 클래스```가 ```Man 클래스```를 상속했기에 나타나는 동작이다.    
그리고 이와 같은 동작 때문에 일반적으로 메소드를 호출하는 것과 같이   
```tellYourInfo()```메소드에서 ```tellYourName()```메소드를 호출할 수 있는 것이다.  
   
이제 간단하게 상속에 관한 언어에 대해서 알아보자
  
* 상속의 대상이 되는 클래스 : 상위 클래스, 기초 클래스, 부모 클래스
* 상속을 받는 클래스 : 하위 클래스, 유도 클래스, 자식 클래스
  
주로 '상/하위 클래스'와 '부모/자식 클래스'를 많이 사용한다.      

## 1.3. 상속과 생성자
상속 관계에서 주의를 기울여야 하는 부분이 있다. 그것은 바로 **생성자**이다.  

```
class Man{
  String name;
  public void tellYourName(){
    System.out.println("My name is " + name);
  }
}

class BusinessMan extends Man{
  String company;
  String position;
  
  public BusinessMan(String company, String position){
     this.company = company;
     this.position = position;
  }
  
  public void tellYourInfo(){
    System.out.println("My company is " + company);
    System.out.println("My position is " + position);
    tellYourName();
  }
}
```
위 코드는 앞선 예시에 대해서 생성자를 만들어 주었을 뿐이다.  
하지만 위 코드를 그대로 사용하면 문제가 하나 있다.    
바로 ```Man 클래스```의 ```name 인스턴스 변수```에 대해서 초기화를 진행하지 않았기 때문이다.  

```
         _____________________________
man ->  |String name                  |
        |String company               |
        |String position              |
        |void tellYoutName            |
        |void tellYoutInfo            |
        |_____________________________|
```
앞서 ```Man 클래스```를 상속한 ```BusinessMan의 인스턴스```는 위와 같은 구조를 한다.      
하지만 앞선 코드의 생성자를 사용하면 ```name 변수```에 대한 초기화를 진행하지 않았기에     
```tellYourInfo()```나 ```tellYourName``` 사용시에 ```name 변수```에 ```null``` 값이 출력 될 것이다.    
  
```
public BusinessMan(String name, String company, String position){
   this.name = name;
   this.company = company;
   this.position = position;
}
  // 사용 가능하지만 적절한 코드는 아니다.
```
위 코드는 사용이 가능하지만 사실 적절한 코드는 아니다.      
그 이유는 사실상 ```name 변수```는 ```BusinessMan 인스턴스 변수```가 아니고 ```Man의 인스턴스 변수```이다.    

생성자의 호출은 해당 클래스를 통해 인스턴스를 생성할 때 호출되는 것인데 
상위 클래스를 상속받는 하위 클래스의 인스턴스를 생성한다고 가정시에
명시적으로 상위 클래스의 생성자 호출하는 것을 정의하지 않으면  
자동으로 상위 클래스의 생성자를 먼저 호출하고 하위 클래스의 생성자를 호출하게 된다.  

만약  ```Man 클래스```에 생성자가 존재하고 ```BusinessMan 인스턴스```를 생성한다고 가정을 하면  
우리 눈에 보이지 않게 ```Man 클래스```의 생성자가 호출되어 사용될 것이다.  
프로그램에 있어서 눈에 보이지 않는 동작을 일일이 기억하기보다는 명시적으로 이를 나타내 주는 것이 좋다.  
그렇기에 ```BusinessMan 클래스```에서 ```Man```의 생성자를 호출하여 명시적으로 사용한다는 것을 나타내주자.      
   
상위 클래스의 생성자를 호출하는 방법은 **super()** 를 사용하면 된다.  
```
public BusinessMan(String name, String company, String position){
   super(name);
   this.company = company;
   this.position = position;
}
```
   
## 1.4. 단일 상속만을 지원하는 자바
자바는 프로그램이 과도하게 복잡해지는 것을 막기 위해 단일 상속만을 지원한다.  

```
class AAA {...}
class MMM extends AAA {...}
class ZZZ extends MMM {...}
```
자바에서 상속은 한개의 클래스만 상속 받을 수 있다.    

### 1.2.1. 내용1
```
내용1
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
