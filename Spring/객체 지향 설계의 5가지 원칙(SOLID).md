## **객체 지향 설계의 5가지 원칙(SOLID)**

SOLID는 아래 5가지 원칙의 앞 머리 알파벳을 따서 부르는 이름이다. 이 원칙은 응집도는 높이고, 결합도는 낮추라는 고전 원칙을 객체 지향의 관점에서 재정립한 것이라고 할 수 있다.

- 단일 책임 원칙(SRP)
- 개방-폐쇄 원칙(OCP)
- 리스코프 치환 원칙(LSP)
- 인터페이스 분리 원칙(ISP)
- 의존관계 역전 원칙(DIP)

<br>

> **결합도와 응집도란?**

모듈이란 소프트웨어 내의 프로그램 또는 작업 단위의 의미로 사용되며 모듈화란 소프트웨어를 각 기능별로 나누는 것을 말한다. 여기서 좋은 모듈화란 각각의 모듈은 주어진 기능을 독립적으로 수행하며 다른 모듈과 적게 연관되어야 한다.

즉, 독립성이 높은 모듈일수록 좋다. 독립성이 높으면 해당 모듈을 수정하더라도 다른 모듈에 끼치는 영향이 적으며 오류가 발생하더라도 쉽게 해결할 수 있기 때문이다.

모듈의 독립성은 결합도(Coupling)과 응집도(Cohesion)로 측정한다. 모듈의 독립성을 높이기 위해서는 결합도를 낮추고 응집도를 높여야 한다.

**결합도**는 모듈(클래스)간의 상호 의존하는 정도를 의미한다. 자바의 클래스로 예를 들면, 결합도가 높은 클래스는 다른 클래스와 연관된 정도가 높기 때문에 해당 클래스 변경 시 연관된 클래스도 변경해야하고 재사용하기 어렵다. 결합도가 낮으면 모듈 간의 상호 의존성이 줄어들어 수정이나 유지보수에 용이하다.

**응집도**는 하나의 모듈 내의 구성요소끼리 서로 관련되어 있는 정도를 말한다. 응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아져 재사용이나 수정에 용이하다.

<br>

### 1. **단일 책임 원칙**

역할(책임)을 분리하라는 것이 단일 책임 원칙이다. 이 원칙을 지킴으로써 몇 가지 이점이 있다.

- 책임이 하나인 클래스는 테스트 케이스가 적다.

- 단일 클래스의 기능이 적을수록 종속성이 줄어들어 결합도가 낮아진다.

단일 책임 원칙은 속성과 메소드, 패키지, 모듈 등에도 적용할 수 있는 개념이다. 다음 예시는 단일 책임 원칙을 위반한다.

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

다음 예시는 메소드가 단일 책임 원칙을 위반한다. Dog 클래스의 소변보다() 메소드가 수컷 강아지의 행위와 암컷 강아지의 행위를 모두 구현하려고 하기에 단일 책임 원칙을 위배하고 있는 것이다. 메소드가 단일 책임 원칙을 지키지 않을 경우는 대표적으로 분기 처리를 위한 if 문이 있다.

```java
class Dog {
  final static Boolean 수컷 = true;
  final static Boolean 암컷 = false;
  Boolean 성별;

  void 소변보다() {
    if(this.성별 == 수컷) {
      //한쪽 다리를 들고 소변을 본다
    } else {
      // 뒷다리 두 개로 앉은 자세로 소변을 본다
    }
  }
}
```

이런 경우 단일 책임 원칙을 적용해 코드를 리팩터링 하면 다음과 같이 만들 수 있다. Dog라는 추상 클래스를 두고 수컷, 암컷강아지 클래스가 자신의 특징에 맞게 소변보다() 메소드를 구현하여 사용한다.

```java
abstract class Dog {
  abstract void 소변보다() 
}

class 수컷강아지 extends Dog {
  void 소변보다() {
    //한쪽 다리를 들고 소변을 본다
  }
}

class 암컷강아지 extends Dog {
  void 소변보다() {
    //뒷다리 두 개로 앉은 자세로 소변을 본다
  }
}
```
<br>

### 2. **개방-폐쇄 원칙**

확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

> 확장이란? 새로운 타입을 추가함으로써 새로운 기능을 구현하는 것

> 변경이란? 확장이 발생했을 때 코드를 호출하는 쪽에서 변경이 발생하지 않는 것

개방-폐쇄 원칙의 좋은 예는 JDBC다. JDBC를 사용하는 클라이언트는 데이터베이스가 오라클에서 MySQL로 바뀌더라도 Connection을 설정하는 부분 외에는 따로 수정할 필요가 없다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/JDBC.png)


다음 예시는 개방-폐쇄 원칙을 위반한 예이다. 운전자의 차 종이 바뀔때마다 새로운 drive() 메소드를 실행해야 한다.

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

### 3. **리스코프 치환 원칙**

객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 클래스의 인스턴스는 상위 객체 참조 변수에 대입해 상위 클래스의 인스턴스 역할을 하는 데 문제가 없어야 한다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/LSP.png)

위 계층도인 경우 딸이 아버지, 할아버지의 역할을 하는 것이 논리에 맞지 않음을 알 수 있다. 아래 계층도의 경우는 리스코프 치환 원칙을 잘 지키고 있다. 하위에 존재하는 것들은 상위에 있는 것들의 역할을 하는 데 전혀 문제가 없다. 고래가 포유류 또는 동물의 역할을 하는 것은 전혀 문제가 되지 않는다.

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
        System.out.println(test.work(new Rectangle())); //true
        System.out.println(test.work(new Square())); //false
    }
}
```

work() 메소드는 상위 타입인 Rectangle를 사용한다. 리스코프 치환 원칙에 따르면 Rectangle을 Square 객체로 치환하여도 프로그램은 정상적으로 동작하여야 한다. 하지만 부모 클래스인 Rectangle의 결과 값은 true이고, Square은 false를 반환한다. 자식 클래스인 Square가 부모클래스의 getArea() 메소드를 제대로 수행하지 못하고 있기 때문이다.

<br>

### 4. **인터페이스 분리 원칙**

단일 책임 원칙에서 제시한 해결책은 클래스를 나누어 하나의 역할(책임)만 하도록 다수의 클래스로 분할하는 것이었다. 인터페이스 분리 원칙은 중간에 인터페이스를 추가하여 역할을 제한하는 것이 핵심이다. 결론적으로 단일 책임 원칙과 인터페이스 분리 원칙은 같은 문제에 대한 두 가지 다른 해결책이라고 볼 수 있다. 하지만 특별한 경우가 아니라면 단일 책임 원칙을 적용하는 것이 더 좋은 해결책이다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/ISP.png)


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

### 5. **의존관계 역전 원칙**

고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다. 이 두 모듈 모두 다른 추상화된 것에 의존해야 한다. 

> 고수준 모듈이란? 인터페이스와 같은 객체의 형태나 추상적 개념

> 저수준 모듈이란? 구현된 객체

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/DIP1.png)

자동차와 스노우타이어 사이에는 그림처럼 의존 관계가 있다. 자동차가 스노우타이어에 의존한다. 이런 경우 스노우타이어를 일반타이어로 교체할 때 자동차는 그 영향에 노출되어 있음을 알 수 있다.

![img5](https://github.com/dilmah0203/TIL/blob/main/Image/DIP2.png)

위와 같이 자동차가 구체적인 타이어들(스노우타이어, 일반타이어, 광폭타이어)이 아닌 추상화된 타이어 인터페이스에만 의존하게 함으로써 타이어가 변경되더라도 자동차는 그 영향을 받지 않는 형태로 구성된다. 

자동차는 자신보다 변하기 쉬운 스노우타이어에 의존하던 관계를 중간에 추상화된 타이어 인터페이스를 추가해 두고 의존 관계를 역전시키고 있다. 이처럼 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 것이 의존 역전 원칙이다.

정리하자면 상위 클래스일수록, 인터페이스일수록, 추상클래스일수록 변하지 않을 가능성이 높기에 하위 클래스나 구체 클래스가 아닌 상위 클래스, 인터페이스, 추상 클래스를 통해 의존하라는 것이다.

<br>

참고

https://www.baeldung.com/solid-principles(https://www.baeldung.com/solid-principles)

