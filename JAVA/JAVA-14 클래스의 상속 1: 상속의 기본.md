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
