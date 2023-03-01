## 인터페이스 vs 추상클래스 

추상화란, 공통된 특징을 묶어 하나의 클래스로 정의하는 것이다. Java는 추상화를 구현하기 위해 추상클래스와 인터페이스 두가지 기능이 존재한다.

**인터페이스와 추상클래스의 공통점과 차이점**

- 인터페이스와 추상클래스를 인스턴스화 하는 것은 불가능하며, 구현부가 있는 메소드와 없는 메소드를 모두 가질 수 있다는 점에서 유사하다.
- 인터페이스에서 모든 변수는 기본적으로 public static final이며, 모든 메소드는 public abstract인 **반면** 추상클래스는 static이나 final이 아닌 변수를 지정할 수 있고 public, protected, private 메소드를 가질 수 있다.
- 인터페이스는 다른 여러개의 인터페이스들을 함께 구현할 수 있지만, 자바는 다중 상속을 지원하지 않으므로 추상클래스를 상속받은 서브클래스는 다른 클래스를 상속받을 수 없다.

## 인터페이스

인터페이스의 경우 default 또는 static으로 선언되지 않은 메소드는 모두 abstract이기 때문에 생략이 가능하다.

```java
public interface 인터페이스이름 {

    //상수 필드
    //메소드
}
```

> 언제 사용할까?

- 관련 없는 클래스가 인터페이스를 구현할 때
- 특정 데이터 타입의 행동을 명시하고싶은데, 어디서 구현되는지 상관 없는 경우
- 다중상속을 활용할 때

첫 번째 관련 없는 클래스가 인터페이스를 구현할 경우는, 두 객체를 비교하기 위해 Comparable 인터페이스의 compareTo() 메소드를 재정의하는 경우가 있다.

```java
public class Car {

    public void method() {
        System.out.println("car method");
    }
}
  
public class Main {

    public static void main(String[] args) {
    
        Car car = new Car();
        car.method();
    }
}
```

Main 클래스에서 `Car car = new Car();` 과같이 객체를 생성하였다. 객체 생성 이후 Car 클래스에 선언된 모든 메소드를 사용할 수 있게 된다. 하지만 Car 클래스가 변경되면 Main 클래스도 변경해주어야 한다. 이것은 Car 클래스와 Main 클래스가 강한 결합을 이루고 있다고 볼 수 있으며 인터페이스를 이용하여 다음과 같이 결합도를 낮출 수 있다.

```java
public class Example {

    void auto(Movable m) {
        m.method();
    }
}

public interface Movable {

    public void method();
}

public class Car implements Movable {

    @Override
    public void method() {
        System.out.println("method in Car class");
    }
}

public class Bus implements Movable {

    @Override
    public void method() {
        System.out.println("method in Bus class");
    }
}

public class Main {

    public static void main(String[] args) {
    
        Example e = new Example();
        e.auto(new Car()); //method in Car class
        e.auto(new Bus()); //method in Bus class
    }
}
```

Movable 인터페이스를 구현하는 Car과 Bus 클래스는 각각 다른 동작을 정의하였다. Main 클래스는 동작 변경을 위해 Movable 인터페이스를 구현하는 객체를 바꿔주기만 하면 된다. 

## 추상클래스

추상 메소드는 아래와 같이 구현부가 없는 메소드를 말하며, 추상클래스를 상속받는 클래스는 상위클래스에 있는 모든 추상메소드들을 구현한다. 그렇지 않은 경우 해당 서브클래스 또한 abstract로 선언되어야 한다.

```java
public abstract void method();
```

```java
public abstract class 클래스이름 {

    //필드 
    //추상메소드
    
    //일반메소드
    public void move() {
    }
}
```

> 언제 사용할까?

- 여러 하위 클래스의 공통 기능을 캡슐화할 때
- 접근제한자가 public이 아닌 공통적인 필드나 메소드를 가지는 클래스를 상속받고자 할 때 
- 상태 변경을 위해 non-static, non-final 필드 선언이 필요할 때

```java
public abstract class Shape {

    int x;
    
    public void move() {
        ...
    }
    
    abstract void draw();
    abstract double area();
}

public class Rectangle extends Shape {

    @Override
    public void draw() {
        ...
    }
    
    @Override
    public double area() {
        ...
    }
}

public class Triangle extends Shape {

    @Override
    public void draw() {
        ...
    }
    
    @Override
    public double area() {
        ...
    }
}
```

추상클래스는 **Is a** 관계일 때 사용한다.

- Rectangle is a Shape
- Triangle is a Shape  

인터페이스는 행동을 정의할 수 있지만 상태를 가질 수 없다. 하지만 추상클래스는 `int x;`를 선언함으로써 **상태를 가질 수 있다**는 것이 인터페이스와의 큰 차이점이다. 

## JDK 8의 인터페이스에 추가된 기능

- **default method**

JDK 8 이전엔 새로운 메소드를 추가하려면 인터페이스를 구현하는 모든 클래스들이 해당 메소드를 구현해야했다. 하지만 default method로 추가가 가능해지면서 기존 인터페이스를 구현했던 클래스에서 메소드를 override하지 않아도 된다. 기존 코드에 영향을 주지 않고 새로운 메소드를 가질 수 있도록 이전 인터페이스에 대한 호환성을 제공한다.

```java
public interface Calculator {

    static final int first = 10; //상수 필드
    
    public int plus(int x, int y); 
    public int minus(int x, int y);
    
    default int multiply(int x, int y) {
        return x * y;
    }
}

public class Example implements Calculator {

    @Override
    public int plus(int x, int y) {
        return x + y;
    }
    
    @Override
    public int minus(int x, int y) {
        return x - y;
    }
}

public class Main {

    public static void main(String[] args) {
    
        Calculator calc = new Example();
        int value = calc.multiply(2, 3);
        System.out.println(value); //6
    }
}
```

- **static method**

객체 생성 여부와 상관없이 `Calculator.multiply();`와 같이 인터페이스만으로도 호출이 가능하며 override가 불가능하고 상속되지 않는다. 상수를 선언할 때는 static block에서 초기화할 수 없고, 선언과 동시에 초기화해야한다.
  
```java
public interface Calculator {

    static final int first = 10;
    
    public int plus(int x, int y); 
    public int minus(int x, int y);
    
    static int multiply(int x, int y) {
        return x * y;
    }
}
```

<br>

Java는 추상화를 구현하기 위해 추상클래스와 인터페이스 두가지 기능이 존재한다. **인터페이스**는 상태를 가질 수 없지만 동작 변경을 통해 결합도를 낮추는 것을 도와준다. **추상클래스**는 상태를 가질 수 있고 메소드 오버라이딩을 통한 공통 기능을 구현하고 확장시킬 때 사용한다.

<br>

참고

[https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)

[https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)

[https://www.geeksforgeeks.org/difference-between-abstract-class-and-interface-in-java/](https://www.geeksforgeeks.org/difference-between-abstract-class-and-interface-in-java/)

[https://www.journaldev.com/1607/difference-between-abstract-class-and-interface-in-java](https://www.journaldev.com/1607/difference-between-abstract-class-and-interface-in-java)


