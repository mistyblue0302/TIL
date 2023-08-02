## 정적 팩토리 메소드(Static Factory Method) 

```java
public class Book {

    private String title;

    private Book(String title) {
        this.title = title;
    }

    public static Book titleOf(String title) { //정적 팩토리 메소드
        return new Book(title); //메소드에서 생성자를 호출하여 리턴
    }

    public static void main(String[] args) {
        Book b = Book.titleOf("abc"); //정적 팩토리 메소드 호출을 통해 객체를 얻는다.
    }
}
```

정적 팩토리 메소드는 다음과 같은 장단점이 있다.

- 장점
    - 이름을 가질 수 있어 생성자에 비해 가독성이 좋다.
    - 호출될 때마다 객체를 새로 생성하지 않아도 된다.
    - 하위 자료형 객체를 반환할 수 있다.
    - 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
- 단점
    - 정적 팩토리 메소드만 제공하면, 생성자가 없으므로 하위 클래스를 만들 수 없다.
    - 정적 팩토리 메소드는 찾기 어렵다. (다른 메소드와 잘 구분되지 않는다.)

### 1. 이름 표현을 통해 반환될 객체를 쉽게 묘사할 수 있다.

다양한 타입의 객체를 생성하기 위해, 생성 목적에 따라 생성자를 오버로딩하여 구분하여 사용했다. 하지만 new 키워드를 통해 생성자로 생성하면,
생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 제대로 설명하지 못한다.

```java
public class Character {

    int intelligence, strength, hitPoint, magicPoint;

    public Character(int intelligence, int strength, int hitPoint, int magicPoint) {
        this.intelligence = intelligence;  
        this.strength = strength;           
        this.hitPoint = hitPoint;           
        this.magicPoint = magicPoint;       
    }

    //정적 팩토리 메소드
    public static Character newWarrior() {
        return new Character(5, 15, 20, 3);     //전사는 힘과 HP가 높다.
    }

    //정적 팩토리 메소드
    public static Character newMage() {
        return new Character(15, 5, 10, 15);    //마법사는 지능과 MP가 높다.
    }
}
```

만약 생성자를 사용해 전사나 마법사를 생성한다면 다음과 같을 것이다.

```java
Character warrior = new Character(8, 20, 30, 3);
Character mage = new Character(20, 3, 10, 13);
```

변수명으로 구분짓지 않는다면 연속된 숫자만으로 캐릭터의 직업을 유추하기 어렵다.

하지만 정적 팩토리 메소드를 사용해 적절한 메소드 네이밍을 해준다면 반환될 객체의 특성을 유추하기 쉬워진다.

```java
Character warrior = Character.newWarrior();
Character mage = Character.newMage();
```

### 2. 호출할 때마다 새로운 객체를 생성할 필요가 없다.

위 코드는 정적 팩토리 메소드를 호출할 때마다 `new Character()`를 호출하게 된다. 근데 immutable 객체를 미리 만들어노고
재활용하는 식으로 불필요한 객체 생성을 피한다면(특히 생성 비용이 큰) 성능을 끌어올려 준다.

다음은 `java.math.BigInteger`의 코드이다.

```java
public static final BigInteger ZERO = new BigInteger(new int[0], 0);

private final static int MAX_CONSTANT = 16;
private static BigInteger posConst[] = new BigInteger[MAX_CONSTANT+1];
private static BigInteger negConst[] = new BigInteger[MAX_CONSTANT+1];

static {
    //posConst에 1 ~ 16까지의 BigInteger 값을 담는다. 
    //negConst에 -1 ~ -16까지의 BigInteger 값을 담는다. 
}

public static BigInteger valueOf(long val) {
        if (val == 0)
            return ZERO;
        if (val > 0 && val <= MAX_CONSTANT)
            return posConst[(int) val];
        else if (val < 0 && val >= -MAX_CONSTANT)
            return negConst[(int) -val];

        return new BigInteger(val);
}
```

위 방법을 사용하면 `0`값은 호출시마다 일일이 객체를 생성하는 일을 피할 수 있다.

### 3. 하위 자료형 객체를 반환할 수 있다.

메소드 호출을 통해 반환할 객체의 타입을 유연하게 지정할 수 있다.

```java
public interface SmartPhone {}

public class Galaxy implements SmartPhone {}
public class IPhone implements SmartPhone {}

public class SmartPhones {
    public static SmartPhone getSamsungPhone() {
        return new Galaxy();
    }

    public static SmartPhone getApplePhone() {
        return new IPhone();
    }
}
```

하지만 java 8부터는 인터페이스가 정적 메서드를 가질수 있게 되어 인터페이스에 그냥 정적 팩토리 메소드를 선언하면 된다.

```java
public interface SmartPhone {

    public static SmartPhone getSamSungPhone() {
        return new Galaxy();
    }

    public static SmartPhone getApplePhone() {
        return new IPhone();
    }
}
```

### 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

메서드 블록 내에서 분기문을 통해 여러 자식 타입의 인스턴스를 반환하도록 응용 구성이 가능하다. 반환 타입의 하위 타입이기만 하면
어떤 클래스의 객체를 반환하든 상관없다.

```java
public interface SmartPhone {
    public static SmartPhone getPhone(int price) {
        if(price > 100000) {
            return new IPhone();
        }
        return new Galaxy();
    }    
}
```

### Lombok의 RequiredArgsConstructor 

Lombok의 `RequiredArgsConstructor`를 사용하면 정적 팩토리 메소드를 쉽게 만들 수 있다.

```java
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor(staticName = "of")
public class BlogUser {
    private final Long id;
    private final String name;
}
```

위와 같이 staticName을 사용하면 lombok이 of라는 이름을 가진 정적 팩토리 메소드를 만들어준다. 즉, 다음과 같이 사용할 수 있다.
