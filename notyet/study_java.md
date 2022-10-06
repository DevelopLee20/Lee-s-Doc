# Java 언어 기본

## 자바 기본 클래스

```java
public class main{
    public static void main(String[] args){
        코드 내용
    }
}
```

---

## 문자 출력 방식

```java
System.out.println("Hello Java");
```

---

## 변수 선언 방식

```java
int number;
number = 10;
int number = 10;
```

---

## 자료형이 다른 변수끼리 연산

```java
short A = 10; // 2바이트 정수형
int B = 20; // 4바이트 정수형

System.out.println(A+B); // 결과값 30
```

### 정수형들은 연산시에 4바이트를 기본 단위로 사용하기 때문에 서로 다른 데이터형이어도 상관없다. 또한 저장되는 값 또한 정수형으로 저장된다

---

## 문자형 대입 방법

```java
char A = 'A';
char B = 66;

System.out.println(A); // 출력 : A
System.out.println(B); // 출력 : B
```

---

## 데이터형 변환 방법

```java
int A = 65;

System.out.println(A); // 출력 : 65
System.out.println((char)A); // 출력 : A
```

---

## 부동소수점 식별자

```java
double a = 3.14;
float b = 3.14F;
float c = 3.14f;

System.out.println(a); // 3.14
System.out.println(b); // 3.14
System.out.println(c); // 3.14
```

> 실수형은 기본적으로 double 형이다.  그래서 float 형을 선언해서 초기화 할때는 double 형이 아닌 float 형으로 선언하겠다는 F or f 식별자를 표시해주어야 한다

---

## 리스트, 배열의 길이

```python
#python
s = []
len(s)
```

```java
//java
int[] s = new int[10];
int[] score = {10,20,30,40};
s.length;
```

---

## 배열 인자 하나씩 호출

```python
#python
s = [1,2,3,4,5,6]
for i in s:
    print(s)
```

```java
//java
int[] s = {1,2,3,4,5};
for (int value : s){
    System.out.println(value+" ");
}
```

---

## 배열의 모든 요소 출력

```python
#python
s = [10,20,30]
print(s)
```

```java
//java
int[] s = {10,20,30};
System.out.println(Arrays.toString(number));
```

---

## 자바의 ArrayList = 파이썬의 list와 같다

```java
import java.util.*;

public class First_java{
    public static void main(String args[]) {
        ArrayList<String> list=new ArrayList<>();
        list.add("인규");
        list.add("감자");
        list.add("사과");
        for(String obj:list) {
            System.out.print(obj+" ");
        }
    }
}
```

```java
ArrayList list = new ArrayList();//타입 미설정 Object로 선언된다.
ArrayList<Student> members = new ArrayList<Student>();//타입설정 Student객체만 사용가능
ArrayList<Integer> num = new ArrayList<Integer>();//타입설정 int타입만 사용가능
ArrayList<Integer> num2 = new ArrayList<>();//new에서 타입 파라미터 생략가능
ArrayList<Integer> num3 = new ArrayList<Integer>(10);//초기 용량(capacity)지정
ArrayList<Integer> list2 = new ArrayList<Integer>(Arrays.asList(1,2,3));//생성시 값추가
```

---

## 객체지향언어

```java
class Calculator{

    int plus(int a,int b) {
        return a+b;
    }
    int minus(int a, int b) {
        return a-b;
    }
}

public class First_java{
    public static void main(String[] args) {
        Calculator obj1 = new Calculator();

        int one = obj1.plus(10, 20);
        int two = obj1.minus(30, 20);
        System.out.println(one);
        System.out.println(two);
    }
}
```

---

## String[] args 와 String args[] 의 차이점

```java
String[] args
String args[]
```

> <https://stackoverflow.com/questions/13175193/java-array-convention-string-args-vs-string-args>
>
> 결론적으론 없다.

---

## 메소드 오버리딩

```java
class Mymath{
    int square(int i){ // square 라는 메소드 생성
        return i * i;
    }

    double square(double i){ // square 라는 메소드를 중복 생성, 이때 매개변수는 다르게 설정해야한다.
        return i * i;
    }

    int square(){
        return 30 * 30;
    }
}

public class Page_130 {
    public static void main(String[] args){
    
        System.out.println(obj.square(10));
        System.out.println(obj.square(3.14));
        System.out.println(obj.square());
        // 매개변수에 맞는 메소드가 호출되어 사용된다.
    }
}
```

> 메소드 오버리딩을 통해서 중복되는 메소드를 형성할 수 있도록 한다.
>
> 이때 매개변수는 달라야 한다.
>
> 매개변수가 없을 때는 없는 메소드가 호출된다.
>
> 정해진 틀에서 약간씩 바꿔주기 위해서

---

## 문자열 기초 연산

```java
String s = "Java";
int size = s.length(); // 4가 반환됨
char c = s.charAt(0); // J가 반환됨 ( 문자열이름.charAt(인덱스))

String s1 = "Java";
String s2 = "Python";
boolean result = s1.equals(s2); // false 가 반환됨

int index = s1.indexOf("Java"); // 괄호안의 내용이 포함되어 있을경우 index 번호를 반환 없으면 -1을 반환한다.

if (index == -1){
    System.out.println("No Table");
}
else{
    System.out.println(index);
}
```

>자바에서는 == 연산자로 문자열을 비교할 수 없다.
>
>== 연산자는 문자열 객체의 주소만을 비교하게 한다.

---

## 객체 소멸

> 객체를 소멸시키기 위해서는 null 값을 대입하면 된다.

---

## 상속

```java
class Child extends Parent{

}
```

> class 자식클래스 extends 부모클래스
>
> 이렇게 작성하면 부모클래스의 메소드를 물려받는다.

---

## 상속 protected 접근 지정자

```java
class Animal{
    protected int a;
    void eat(){
        System.out.println("eating...");
    }
}

class Dog extends Animal{
    void bark(){
        System.out.println("barking...");
    }
}

public class Page_183{
    public static void main(String[] args){
        Dog d = new Dog();
        d.bark();
        d.eat();
    }
}
```

> 위의 코드에서 int a는 protected로 접근 지정자가 설정되어있기 때문에 자식 클래스가 사용불가능하다.

---

## 상속의 생성자 호출 순서

```java
super();
```

> 상속을 할 경우 (부모 클래스의 생성자) -> (자식 클래스의 생성자) 순으로 호출된다.
>
> super(); 라는 키워드가 부모클래스의 생성자를 명시적으로 호출해준다.

---

## 추상 클래스

```java
public abstract class Animal{ // 추상적인 abstract 클래스 생성
    public abstract void move(); // 내용이 담겨있지 않다.
}

public class Lion extends Animal{
    public void move(){ // 부모 클래스의 추상 클래스의 메소드를 새로 정의해준다.
        System.out.println("move"); // System.out.println();
    }
}
```

> abstract 메모하세요!
>
> 추상 클래스를 자식 클래스가 메소드  재정의 해주지 않으면 오류가 발생함

---

## 실제 객체를 알려주는 코드

```java
Shape s = new Rectangle();

boolean a = (s instanceof Rectangle) // true 일 경우 실제 객체가 Rectangle, 아니면 다른 것이 객체이다.
```

---

## 패키지 Package

```java
package lib;
package main;

lib1.Circle obj1 = new lib1.Circle(); // 패키지 내의 클래스 객체 생성
```

> lib, main 내부의 class 파일에 접근할 수 있도록 한다.

```java
java.applet // 자바 애플릿 생성
java.awt // 그래픽과 이미지
java.io // 입력과 출력 스트림
java.lang // 자바 프로그래밍 언어에 필수적인 클래스
java.math // 수학
java.net // 네트워킹 클래스
java.nio // 새로운 네트워킹 클래스
java.security // 보안 프레임워크를 위한 클래스, 인터페이스
java.sql // 데이터베이스 접근
java.util // 날짜, 난수, scanf 사용을 위한 클래스
javax.imageio // 자바 이미지 API
javax.net // 네트워킹 애플리케이션
javax.swing // 스윙 컴포넌트
javax.xml // XML 지원 패키지
import java.applet.*;
```

---

## 예외처리

```java
try{
    int result = x / y; // 실행 코드
} catch (ArithmeticException e){ // 예외 발생시 실행코드
    System.out.println("0");
}
} finally{ // try 후의 실행 코드 , 예외 발생 x 시 실행코드
    System.out.println("2");
}
```

---

## 람다식

```java
(int a, int b) -> { return a + b; }
```

---

> 이름이 필요없는 익명 함수 = 람다식
