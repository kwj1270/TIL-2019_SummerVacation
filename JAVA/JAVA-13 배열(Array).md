배열(Array)
=======================
# 1. 1차원 배열의 이해와 활용
## 1.1. 1차원 배열의 생성 방법
   
1차원 배열 : 타입이 같은 둘 이상의 데이터를 저장할 수 있는 1차원 구조의 메모리 공간
  
**자바에서는 배열을 인스턴스(객체)로 처리한다.**  
그렇기에 배열을 나타내는 변수는 '참조 변수'이다.  
```
int [] arr = new int[5];
```
  
**배열 생성 방법**
```
기본)

int [] arr1 = new int[5]; // 0~4 까지(5개)의 인덱스를 가진 배열 인스턴스 생성 
_____________________________________________________________________________
선언후 대입)

int [] arr2;
arr2 = new int[5]; // 선언후 대입으로 진행해도 된다.
_____________________________________________________________________________
클래스 배열)

String [] arr3 = new String[3]; // 클래스도 배열로 만들 수 있다.  
_____________________________________________________________________________
```
여기서 중요한 점은 배열을 선언 하는 것은 해당 변수를 선언한 것이지 실제로 값이 있는 무언가를 할당하지는 않았다.      
   
**클래스 배열**에서 중요한 점은 클래스를 참조하는 변수를 만든 것이지 실제로 어떤 것을 참조할지는 값을 주지는 않았다.    
그렇기에 클래스 배열을 선언한 후에도 각각의 인덱스에 ```new 클래스()```를 통해 참조할 대상을 가리켜 주어야한다.   
```
class Box{
  private String conts;
  
  Box(String conts){
    this.conts = conts;
  }
  // 오버라이딩을 통해서 println()시에 출력할 대상 정함 
  public String toString(){
    return conts;
  }
}

class BoxArray{
  public static void main (String[] args){
    Box [] box = new Box[3];

    // 배열의 각 요소에 인스턴스 저장
    box[0] = new Box("first");
    box[1] = new Box("second");
    box[2] = new Box("Third");

    // 배열의 각 요소에 저장된 인스턴스 참조
    System.out.println(ar[0]);
    System.out.println(ar[1]);
    System.out.println(ar[2]);
  }
}
```
배열에는 ```length```라는 상수가 존재한다.  
배열에서 ```length```는 배열의 인덱스의 개수를 의미한다.  
예를 들면 ```Box[] box = new Box[5}```로 선언할 경우 인덱스는 ```[0] ~ [4]```이지만 5개의 변수가 생성된다.    
여기서 말하는 5는 length의 값이 된다. 즉, **배열의 길이가 length** 가 된다.    
    
마지막 인덱스의 값은 ```배열의 길이-1``` 이다.  
우리는 이를 이용해 반복문을 생성하여 코드를 더욱 간략히 나타낼 수 있다.  
```
    for(int i = 0; i < box.length; i++){
      box[i] = new Box();
    }
    ...중략...
    for(int i = 0; i < box.length; i++){
      System.out.println(box[i]);
    }
```
그리고 ```length```와 ```length()```의 차이를 헷갈려하는 경우가 많은데    
```length```는 배열의 길이를 나타내는 상수이고        
```length()```는 **문자열의 길이**를 구하는 메소드이다.    
그리고 ```length()```는 **빈 공백도 문자열의 길이에 포함한다!**  
        
## 1.2. 배열을 생성과 동시에 초기화하기   
배열을 생성하는 방법에는 여러 방법이 있다.      
   
**기본**
```
int[] arr = new int[3] {1, 2, 3};
```
**가변 길이**
```
int[] arr = new int[] {1, 2, 3};
```
**간략히**
```
int[] arr = {1, 2, 3};
```
    
그리고 배열의 참조변수를 선언하는 방법도 2가지가 있다.
   
**기본**
```
int[] arr;
```
**C언어 기반**
```
int arr[];
```
하지만 자바에서는 ```int[] arr;```와 같은 방법을 주로 사용한다.         
   
## 1.3. 배열의 참조 값과 메서드
배열도 인스턴스이므로 메소드 호출 시 참조 값의 전달이 가능하다.      
달리 말하면 메소드를 정의할 때 배열을 매개변수의 자료형으로 지정해도 된다.     
또한, 배열을 반환형의 자료형으로 지정할 수도 있다.     
```
메인)

public static void main(string[] args){
   int[] ar = {1, 2, 3, 4, 5, 6, 7};
   int sum = sumOfAry(ar);
}
_____________________________________________
매개변수)

static int sumOfAry(int [] ar){
   int sum = 0;
   for(int i = 0; i < ar.length; i++ ){
      sum += ar[i];
   }
   return sum;
}
____________________________________________
반환형)

static int[] makeNewIntAry(int len){
   int ar[] = new int[len];
   return ar;
} 
```
   
   
   
## 1.4. 배열의 초기화와 배열의 복사  
배열이 생성될 때 아무런 값을 넣어 주지 않는다면 모든 요소는 0 또는 null로 초기화 된다.  
```
int[] ar = new int[10]; // [0]~[9] 까지 숫자형 자료형이므로 0으로 초기화

String[] ar2 = new Stirng[10]; // [0]~[9] 까지 객체(클래스)이므로 null로 초기화
```
  
### 1.4.1. 원하는 배열의 값을 한번에 저장해주는 fill() 메소드  
수치형이나 객체형의 배열의 값을 하나로 통일 하고자 한다면    
```Arrays```클래스의 **static 메소드인 ```fill()```메소드를 사용하면 된다.**    
여기서 말하는 ```Arrays```클래스 배열과 관련된 클래스라고 생각하면 된다.    
   
**fill() 구조**
```
public static void fill(int[]a, int val){ 생략 }

//오버라이딩
public static void fill(int[]a, fromIndx, int toIndex, int val){ 생략 }
```
```public static void fill(int[]a, fromIndx, int toIndex, int val){ 생략 }```같은 경우는    
```fromIndex ~ toIndex-1```의 범위 까지 val의 값으로 배열을 초기화 시킨다.    

```
int[] ar = new int[10]; // [0]~[9] 까지 숫자형 자료형이므로 0으로 초기화
String[] ar2 = new Stirng[10]; // [0]~[9] 까지 객체(클래스)이므로 null로 초기화

//Arrays 의 static 메소드(클래스 메소드)
Arrays.fill(ar, 3);  // [0]~[9] 까지 3으로 채움
Arrays.fill(ar, 0, 9, 2); // [0]~[8] 까지 2로 채움 
//현재 [2][2][2][2][2][2][2][2][3]

//Arrays 의 static 메소드(클래스 메소드)
Arrays.fill(ar2, 'b');        // [0]~[9] 까지 'b'로 채움  
Arrays.fill(ar2, 0, 9, 'a');  // [0]~[8] 까지 'a'로 채움  
//현재 ['b']['b']['b']['b']['b']['b']['b']['b']['b']['a']
```

### 1.4.2. 배열의 값을 복사하는 arraycopy() 메소드
배열은 일반 변수처럼 ```int[] arr2 = arr1``` '배열 = 배열'로 대입을 할 수가 없다.   
그렇기에 배열을 복사하고자 한다면 반복문을 통해서 값들을 일일이 넣어주어야 하는데  
이를 간략하게 메서드로 모듈화한 것이 있는데 그것이 ```arraycopy()```메소드 이다.
    
```arraycopy()```메소드는 **```System```클래스의 satic 메소드이다.**
   
**arraycopy() 구조**
```
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length){생략}
```
```srcPos``` 는 배열```src```의 시작 인덱스를 나타낸다.  
```
int[] test2 = new int[10];
		Arrays.fill(test2, 7);
		
		int[] test3 = new int[10];
		Arrays.fill(test3, 0);
		
		test2[0] = 1;
		
		System.arraycopy(test2, 0, test3, 3, 4);
		
		for(int i = 0 ; i < test3.length; i++) {
			System.out.print(test3[i]+" ");
		}
```
위 코드를 보면 text2는 1,7,7,7,7,7,7,7,7,7의 값을 가진다.  
```System.arraycopy(test2, 0, test3, 3, 4);```는   
test2의 [0]번 인덱스부터의 값을 test3 인덱스의 [3]번 인덱스에 넣는데 그 개수는 4개로 제한한다는 의미이다.     




