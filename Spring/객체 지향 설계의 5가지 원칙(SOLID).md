# 💡 **객체 지향 설계의 5가지 원칙(SOLID)**

이러한 다섯가지 원칙은 소프트웨어 개발과 유지보수를 쉽게 할 수 있도록 한다.

- 단일 책임 원칙
- 개방-폐쇄 원칙
- 리스코프 치환 원칙
- 인터페이스 분리 원칙
- 의존관계 역전 원칙

<br>

1. **단일 책임 원칙**

한 클래스는 하나의 책임만 가져야 한다. 이 원칙을 지킴으로써 몇 가지 이점이 있다.

- 책임이 하나인 클래스는 테스트 케이스가 적다

- 단일 클래스의 기능이 적을수록 종속성이 줄어들어 결합도가 낮아진다

다음 예시는 단일 책임 원칙을 위반한다.

```java
public class Book {

  private String name;
  private String author;
  private String text;

  //constructor, getters and setters

  public String isWordInText(String word) {
    return text.contains(word);
  }

  public void printTextToConsole() {

  }
}
```
isWordInText() 메소드와 printTextToConsole() 메소드는 문자열 포함 여부를 확인하고 텍스트를 출력하는 서로 다른 동작을 한다.

다음과 같이 텍스트 출력만 처리하는 별도의 클래스를 구현함으로써 출력과 관련 없는 Book 클래스를 들릴 일이 없어져 클래스 간의 결합도가 줄어들게 된다.

```java
public class BookPrinter {

  public void printTextToConsole() {

  }
}
```
<br>

2. **개방-폐쇄 원칙**

확장에는 열려 있으나 변경에는 닫혀 있어야 한다.


**확장이란?**

새로운 타입을 추가함으로써 새로운 기능을 구현하는 것

**변경이란?**

확장이 발생했을 때 코드를 호출하는 쪽에서 변경이 발생하지 않는 것


다음 예시는 운전자의 차 종이 바뀔때마다 새로운 drive() 메소드를 실행해야 한다.

```java
public class Driver {

  public static void main(String[] args) {

      Truck truck = new Truck(); 
      truck.drive(); 
      Bus bus = new Bus(); 
      bus.drive(); 
  } 
} 

class Truck { 
  public void drive() {
     System.out.println("Truck Drive");
  } 
} 

class Bus { 
  public void drive() {
    System.out.println("Bus Drive"); 
  }
}
```

이러한 문제를 개방-폐쇄 원칙을 이용하여 구현하면 아래와 같다.

```java
public class Driver { 

  public static void main(String[] args) {

     Car[] car = new Car[2]; 
     car[0] = new Truck(); 
     car[1] = new Bus(); 

     for (int i = 0; i < car.length; i++) {
        car[i].drive(); 
     } 
  } 
} 

class Car { 

  public String carType = "";

   public void car(String carType) {
      this.carType =carType;
   } 
   
   public void setCarType(String carType) {
      this.carType =carType;
   } 
   
   public void drive() {
      System.out.println(carType + " Drive");
   }
} 

class Truck extends Car {

  public Truck() { 
    setCarType("Truck");
  } 
  
  @Override 
  public void drive() { 
    super.drive(); 
  } 
} 

class Bus extends Car {

  public Bus() { 
     setCarType("Bus");
  } 
  
  @Override 
  public void drive() {
     super.drive(); 
  }
}
```

Car라는 클래스에서 drive() 메소드를 선언함으로써 어떤 차종이 와도 drive()를 오버라이딩할 수 있다. 따라서 Car 클래스의 코드 변경 없이 drive() 기능을 확장할 수 있다.

<br>

3. **리스코프 치환 원칙**

객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

너비와 높이를 가지는 Rectangle 클래스가 있다.

```java
public class Rectangle {

    protected int width;
    protected int height;

    public int getHeight() {
        return this.height;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getWidth() {
        return this.width;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public int area() {
        return this.width * this.height;
    }
}
```

Square 클래스는 Rectangle을 상속받고 있으며 setHight()와 setWidth()를 오버라이딩 하였다.

```java
public class Square extends Rectangle {

    @Override
    public void setHeight(int value) {
        this.width = width;
        this.height = value;
    }

    @Override
    public void setWidth(int value) {
        this.height = height;
        this.width = value;
    }
}
```

```java
public class Test {

    public boolean work(Rectangle rectangle) {
        rectangle.setHeight(5);
        rectangle.setWidth(3);

        return rectangle.area() == 15;
    }

}

public class Main {

    public static void main(String[] args) {
        Test test = new Test();
        System.out.println(test.work(new Rectangle()));
        System.out.println(test.work(new Square()));
    }
}
```

work() 메소드는 상위 타입인 Rectangle를 사용한다. 리스코프 치환 원칙에 따르면 Rectangle을 Square 객체로 치환하여도 프로그램은 정상적으로 동작하여야 한다. 하지만 부모 클래스인 Rectangle의 결과 값은 true이고, Square은 false를 반환한다. 자식 클래스인 Square가 부모클래스의 getArea() 메소드를 제대로 수행하지 못하고 있기 때문이다.

<br>

4. **인터페이스 분리 원칙**

특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다. 인터페이스가 명확해지고, 대체 가능성이 높아진다.

인터페이스는 클라이언트 입장에서 사용하는 기능만 제공하도록 인터페이스를 분리해야 한다. 이로써 한 기능에 대한 변경의 여파를 최소화할 수 있다.

```java
public interface multifunction {

  void copy();
  void fax(Address from, Address to);
  void print();
}

public class CopyMachine implements multifunction {

  @Override
  public void copy() {
  }

  @Override
  public void fax(Address from, Address to) {
  }

  @Override
  public print() {
  }
}
```

multifunction 인터페이스에 모든 기능을 넣었더니, CopyMachine을 구현하는데 필요없는 fax()와 print() 메소드도 구현을 해줘야 한다.

인터페이스 분리 원칙을 사용하면 다음과 같이 작성할 수 있다.

```java
public interface Print{
  void print();
}

public interface Copy {
  void copy();
}

public interface Fax {
  void fax(Address from, Address to);
}

public class CopyMachine implements Copy {
  @Override
  public void copy() {
  }
}
```
<br>

5. **의존관계 역전 원칙**

고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다. 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다는 것을 의미한다.

고수준 모듈이란?

인터페이스와 같은 객체의 형태나 추상적 개념

저수준 모듈이란?

구현된 객체

정의는 위와 같다. 위존성 역전 원칙에 대입하면, 객체는 객체보다 인터페이스에 의존해야 한다는 뜻이다.


https://steady-coding.tistory.com/388(https://steady-coding.tistory.com/388)


<br>


참고

https://www.baeldung.com/solid-principles(https://www.baeldung.com/solid-principles)
