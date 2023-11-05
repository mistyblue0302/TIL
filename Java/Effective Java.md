# 목차
<!-- TOC -->

- [1. 생성자 대신 정적 팩토리 메소드를 고려하라](https://github.com/dilmah0203/TIL/new/main/Java#%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%86%8C%EB%93%9Cstatic-factory-method)
- [2. 생성자에 매개변수가 많다면 빌더를 고려하라](https://github.com/dilmah0203/TIL/new/main/Java#%EC%83%9D%EC%84%B1%EC%9E%90%EC%97%90-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EA%B0%80-%EB%A7%8E%EB%8B%A4%EB%A9%B4-%EB%B9%8C%EB%8D%94%EB%A5%BC-%EA%B3%A0%EB%A0%A4%ED%95%98%EB%9D%BC)
- [3. private 생성자나 열거 타입으로 싱글톤임을 보증하라](https://github.com/dilmah0203/TIL/new/main/Java#private-%EC%83%9D%EC%84%B1%EC%9E%90%EB%82%98-%EC%97%B4%EA%B1%B0-%ED%83%80%EC%9E%85%EC%9C%BC%EB%A1%9C-%EC%8B%B1%EA%B8%80%ED%86%A4%EC%9E%84%EC%9D%84-%EB%B3%B4%EC%A6%9D%ED%95%98%EB%9D%BC)
- [4. 인스턴스화를 막으려거든 private 생성자를 사용하라](https://github.com/dilmah0203/TIL/blob/main/Java/%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%ED%99%94%EB%A5%BC%20%EB%A7%89%EC%9C%BC%EB%A0%A4%EA%B1%B0%EB%93%A0%20private%20%EC%83%9D%EC%84%B1%EC%9E%90%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라](https://github.com/dilmah0203/TIL/blob/main/Java/%EB%A6%AC%EC%86%8C%EC%8A%A4%EB%A5%BC%20%EC%A7%81%EC%A0%91%20%EB%AA%85%EC%8B%9C%ED%95%98%EC%A7%80%20%EB%A7%90%EA%B3%A0%20%EC%9D%98%EC%A1%B4%20%EA%B0%9D%EC%B2%B4%20%EC%A3%BC%EC%9E%85%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.md)
- [6. 불필요한 객체 생성을 피하라](https://github.com/dilmah0203/TIL/blob/main/Java/%EB%B6%88%ED%95%84%EC%9A%94%ED%95%9C%20%EA%B0%9D%EC%B2%B4%20%EC%83%9D%EC%84%B1%EC%9D%84%20%ED%94%BC%ED%95%98%EB%9D%BC.md)




## 정적 팩토리 메소드(Static Factory Method) 

정적 팩토리 메소드는 다음과 같은 장단점이 있다.

- 장점
    - 이름을 가질 수 있어 생성자에 비해 가독성이 좋다.
    - 호출될 때마다 객체를 새로 생성하지 않아도 된다.
    - 하위 자료형 객체를 반환할 수 있다.
    - 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
    - 런타임 시 유연하게 동적으로 객체의 클래스를 결정할 수 있다.
- 단점
    - 정적 팩토리 메소드만 제공하면, 생성자가 없으므로 하위 클래스를 만들 수 없다.
    - 정적 팩토리 메소드는 찾기 어렵다. (다른 메소드와 잘 구분되지 않는다.)

### 1. 이름 표현을 통해 반환될 객체를 쉽게 묘사할 수 있다.

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;

    public Character(int strength, int dex, int intelligence) {
        this.strength = strength;
        this.dex = dex;
        this.intelligence = intelligence;
    }
}
```

클래스의 인스턴스를 얻기 위해 `new` 키워드를 사용하여 `strength`, `dex`, `intelligence` 매개변수를 가지는 생성자를 만들어주었습니다.

```java
Character character = new Character(20, 30, 10);
```

만약 생성자를 사용해 캐릭터를 만들어 준다면, 다음과 같을 것입니다. 하지만 `new` 키워드를 통해 생성하면, 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 캐릭터 특성을 제대로 설명하지 못합니다. 변수명으로 구분지어 주지 않는다면 연속된 숫자만으로 캐릭터의 직업을 유추하기 어렵습니다.

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;

    public Character(int strength, int dex, int intelligence) {
        this.strength = strength;
        this.dex = dex;
        this.intelligence = intelligence;
    }

    public static Character makeWizard() { //정적 팩토리 메소드
        return new Character(10, 10, 50);
    }

    public static Character makeWarrior() { //정적 팩토리 메소드
        return new Character(60, 10, 5);
    }
}
```

하지만 정적 팩토리 메소드를 사용해 적절한 메소드 이름을 붙여준다면 반환될 객체의 특성을 쉽게 묘사할 수 있습니다.

```java
Character wizard = Character.makeWizard();
Character warrior = Character.makeWarrior();
```

또, 생성자는 시그니처에 제약이 있습니다. 하나의 시그니처로는 생성자를 하나만 만들 수 있기 때문에 그런 경우에도 정적 팩토리 메소드를 사용하는 것이 유용할 수 있습니다.

```java
public class Character {

    private String name;
    private String password;

    private Character(String name) {
        this.name = name;
    }

    private Character(String password) { //컴파일 오류 : 시그니처가 동일하다.
        this.password = password;
    }

    public static Character withName(String name) {
        return new Character(name);
    }

    public static Character withPassword(String password) {
        Character character = new Character(password);
        character.setPassword(password);
        return character;
    }
}
```

위의 코드에서 생성자는 `String name` 값을 받는 유일한 시그니처를 가지고 있습니다. 하지만 `withName()`과 `withPassword()`라는 두 개의 정적 팩토리 메소드를 사용하면 두 가지 방법으로 `Character` 객체를 생성할 수 있습니다.

```java
Character character = new Character("juseon");
Character character2 = Character.withName("juseon");
Character character3 = Character.withPassword("juseon");
```

첫 번째 방법은 `생성자를 이용해 name이 juseon인` Character 객체를 생성하고 값을 초기화 합니다. 두 번째 방법은 `이름을 사용하여` Character 객체를 생성하거나 `비밀번호를 사용하여` Character 객체를 생성합니다.

### 2. 호출할 때마다 인스턴스를 새로 생성할 필요가 없다.

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;

    private Character(int strength, int dex, int intelligence) {
        this.strength = strength;
        this.dex = dex;
        this.intelligence = intelligence;
    }

    private static final Character WIZARD = new Character(10, 10, 50);
    private static final Character WARRIOR = new Character(60, 10, 5);

    public static Character makeWizard() {
        return WIZARD;
    }

    public static Character makeWarrior() {
        return WARRIOR;
    }
}
```

```java
Character character = new Character(20, 30, 10); //new 키워드로 인스턴스 생성
Character wizard = Character.makeWizard();
Character warrior = Character.makeWarrior();

```

`new` 키워드를 사용하면 인스턴스가 그때 그때 생성되는 반면, 정적 팩토리 메소드는 `static`을 통해 미리 생성된 `Character` 객체를 반환하기 때문에 매번 새로운 인스턴스를 생성하지 않아도 됩니다. 따라서 생성 비용이 큰 객체가 자주 요청되는 상황이라면 성능이 향상될 수 있습니다.

데이터베이스 연결을 예시로 들어보겠습니다.

```java
public class DatabaseConnection {

    private static DatabaseConnection instance;

    private DatabaseConnection() {
        //DB 연결을 수행하는 초기화 코드
    }

    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
}
```

이렇게 매번 데이터베이스 연결을 할 필요 없이, 한 번 연결을 수행하고 재사용할 수 있습니다.

### 3. 반환 타입의 하위 자료형 객체를 반환할 수 있다.

정적 팩토리 메소드를 사용하면 메소드 호출을 통해 얻을 객체의 인스턴스를 선택 가능하게 해줍니다.(다형성) `Wizard`와 `Warrior` 클래스는 `Character` 클래스의 하위 클래스로 `Character` 클래스에 정의된 `makeCharacter()` 메소드를 호출하여 `Character` 타입의 객체를 반환합니다. 이것은 다형성을 활용한 것으로, 정적 팩토리 메소드를 통해 해당 하위 클래스의 객체를 얻을 수 있습니다.

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;

    public Character(int strength, int dex, int intelligence) {
        this.strength = strength;
        this.dex = dex;
        this.intelligence = intelligence;
    }

    public int getStrength() {
        return strength;
    }

    public int getDex() {
        return dex;
    }

    public int getIntelligence() {
        return intelligence;
    }

    public static Character makeCharacter(int strength, int dex, int intelligence) { //정적 팩토리 메소드를 사용하여 객체 생성
        return new Character(strength, dex, intelligence);
    }
}
```

```java
public class Wizard extends Character {

    public Wizard(int strength, int dex, int intelligence) {
        super(strength, dex, intelligence);
    }

    public static Wizard makeWizard(int intelligence) {
        return new Wizard(10, 10, intelligence);
    }
}
```

```java
public class Warrior extends Character {

    public Warrior(int strength, int dex, int intelligence) {
        super(strength, dex, intelligence);
    }

    public static Warrior makeWarrior(int strength) {
        return new Warrior(strength, 10,5);
    }
}
```

```java
Character character = Character.makeCharacter(20, 30, 10);
Wizard wizard = Wizard.makeWizard(60);
Warrior warrior = Warrior.makeWarrior(80);

System.out.println(character.getStrength() + ", " + character.getDex() + ", " + character.getIntelligence()); //20, 30, 10
System.out.println(wizard.getStrength() + ", " + wizard.getDex() + ", " + wizard.getIntelligence()); //10, 10, 60
System.out.println(warrior.getStrength() + ", " + warrior.getDex() + ", " + warrior.getIntelligence()); //80, 10, 5
```

- `Character` 클래스의 정적 팩토리 메소드를 호출하여 `Character` 객체를 생성하고 초기화 합니다.
- `Wizard.makeWizard()`를 호출하면 `Wizard` 클래스의 정적 팩토리 메소드가 실행되고 이 메소드는 상위 클래스인 `Character`의 생성자를 호출하여 초기화 합니다.
- `Warrior.makeWarrior()`은 2번과 같은 원리로 동작하고 상위 클래스인 `Character`의 생성자를 호출하여 초기화 합니다.

정적 팩토리 메소드를 통해 객체 생성에 관한 로직을 숨길 수 있습니다. 클라이언트 코드(main)는 구체적인 객체 생성 방법(`makeCharacter()`)을 알 필요가 없고, 클래스 내부가 변경되어도 영향을 받지 않습니다. 단순히 `makeWizard()` 또는 `makeWarrior()` 메소드를 호출하여 객체를 얻을 수 있습니다.

### 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

```java
public class Character {

    private String name;

    public Character(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void attack() {
        System.out.println(name + "이 공격을 합니다.");
    }

    public static Character createCharacter(String name, String type) {
        if ("warrior".equalsIgnoreCase(type)) {
            return new Warrior(name);
        } else if ("wizard".equalsIgnoreCase(type)) {
            return new Wizard(name);
        }

        return new Character(name);
    }
}
```

```java
public class Wizard extends  Character {

    public Wizard(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println(getName() + " 마법사가 공격을 합니다.");
    }
}
```

```java
public class Warrior extends Character {

    public Warrior(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println(getName() + " 전사가 공격을 합니다.");
    }
}
```

```java
Character character1 = Character.createCharacter("전사1", "warrior");
Character character2 = Character.createCharacter("마법사1", "wizard");

character1.attack(); //전사1 전사가 공격을 합니다.
character2.attack(); //마법사1 마법사가 공격을 합니다.
```

`createCharacter()` 메소드는 name과 type을 매개변수로 받습니다. type이 "warrior"인 경우 `Warrior` 객체를 생성하고, "wizard"인 경우 `Wizard` 객체를 생성합니다. 이렇게 정적 팩토리 메소드를 통해 매개변수 type에 따라 매번 다른 클래스의 객체를 반환할 수 있습니다.

### 5. 정적 팩토리 메소드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

`Character` 클래스는 `makeCharacter()` 메소드로 객체를 생성합니다. 이 메소드는 `type`을 배개변수로 받아 객체의 클래스를 결정합니다. 즉, 반환될 객체가 런타임에 동적으로 결정됩니다.

```java
public class Character {

    private String name;

    public Character(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void attack() {
        System.out.println(name + "이 공격합니다.");
    }

    public static Character makeCharacter(String name, String type) {
        if ("warrior".equalsIgnoreCase(type)) {
            return new Warrior(name);
        } else if ("wizard".equalsIgnoreCase(type)) {
            return new Wizard(name);
        }

        return new Character(name);
    }
}
```

```java
public class Wizard extends Character {

    public Wizard(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println(getName() + " 마법사가 공격합니다.");
    }
}
```

```java
public class Warrior extends Character {

    public Warrior(String name) {
        super(name);
    }

    @Override
    public void attack() {
        System.out.println(getName() + " 전사가 공격합니다.");
    }
}
```

```java
Character warrior = Character.makeCharacter("전사", "warrior");
Character wizard = Character.makeCharacter("마법사", "wizard");

warrior.attack();
wizard.attack();
```

정적 팩토리 메소드를 작성하는 시점에는 반환될 객체의 클래스가 무엇인지 알지 못합니다. 런타임 시 동적으로 객체의 클래스를 결정하고 생성할 수 있습니다. `makeCharacter()` 메소드를 호출할 때 `type` 값을 기반으로 `Character` 클래스의 하위 클래스 중 하나를 반환합니다.

> 장점만 있을까?

### 1. public 또는 protected 생성자 없이 정적 팩토리 메소드만 제공하면, 하위 클래스를 만들 수 없다. 

```java
public class Character {

    private String name;
    private int level;

    private Character(String name, int level) {
        this.name = name;
        this.level = level;
    }

    public String getName() {
        return name;
    }

    public int getLevel() {
        return level;
    }

    //정적 팩토리 메소드
    public static Character createCharacter(String name, int level) {
        return new Character(name, level);
    }
}
```

```java
public class Warrior extends Character {

    private int strength;

    private Warrior(String name, int level, int strength) {
        super(name, level); //오류가 발생한다.
        this.strength = strength;
    }

    public int getStrength() {
        return strength;
    }
}
```

위의 코드에서 `Character` 클래스는 정적 팩토리 메소드를 통해 객체를 생성하고, 생성자는 private으로 숨겼습니다. 그리고 `Warrior`라는 하위 클래스를 만들려고하면 오류가 발생합니다. 이유는 하위 클래스가 부모 클래스의 생성자에 접근할 수 없기 때문입니다.

### 2. 정적 팩토리 메소드는 찾기 어렵다.(다른 메소드와 잘 구분되지 않는다.)

```java
public class Character {

    private String name;

    public Character(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    //정적 팩토리 메소드
    public static Character createCharacter(String name) {
        return new Character(name);
    }

    //일반 메소드
    public void create(String name) {
        System.out.println(name + "이 생성됩니다.");
    }
}
```

`createCharacter()`라는 정적 팩토리 메소드와 `create()`라는 일반 메소드의 이름이 유사합니다. 이렇게 메소드명이 유사할 경우 정적 팩토리 메소드를 찾기 어렵고 가독성이 떨어질 수 있습니다.

---

## 생성자에 매개변수가 많다면 빌더를 고려하라

정적 팩토리 메소드와 생성자에는 제약이 있습니다. 선택적 매개변수가 많을 때 처리하기 어려운 상황이 발생할 수 있습니다. 

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;
    private int healthPoint;

    public Character(int strength, int dex, int intelligence, int healthPoint) {
        this.strength = strength;
        this.dex = dex;
        this.intelligence = intelligence;
        this.healthPoint = healthPoint;
    }

    public static Character createCharacter(int strength, int dex, int intelligence, int healthPoint) {
        return new Character(strength, dex, intelligence, healthPoint);
    }
}
```

```java
Character character1 = new Character(20, 30, 10, 0); //생성자를 사용하여 생성
Character character2 = Character.createCharacter(20, 30, 10, 0); //정적 팩토리 메소드로 생성
```

이 상황에서 `healthPoint`가 0인 경우를 나타내기 위해 `int strength`, `dex`, `intelligence` 세 가지의 매개변수만을 받는 생성자를 추가해야 합니다. 이렇게 선택 매개변수에 대한 모든 가능한 조합을 다루기 위해 다수의 오버로드된 생성자나 정적 팩토리 메소드가 필요해집니다.

선택적 매개변수가 많아지면 가독성을 해치고 코드를 작성하기 어려워집니다. 코드를 읽을 때 각 값의 의미가 무엇인지 헷갈릴 것이고, 매개변수의 개수 또한 확인해보아야 합니다. 만약, 타입이 같은 매개변수가 여러 개 있을 경우 찾기 어려운 버그로 이어질 수 있습니다. 클라이언트가 실수로 매개변수의 순서를 바꾸어도 컴파일러는 감지하지 못하고, 결국 런타임에 버그의 가능성이 높아집니다.

> 선택 매개변수가 많을 때 활용할 수 있는 대안이 있을까?

## 자바빈즈 패턴(JavaBeans Pattern)

- 매개변수가 없는 생성자로 객체를 만든 후, setter 메소드들을 호출해 필요한 필드만 설정하는 방식

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;
    private int healthPoint;

    public Character() {
        //기본 생성자
    }

    public void setStrength(int strength) {
        this.strength = strength;
    }

    public void setDex(int dex) {
        this.dex = dex;
    }

    public void setIntelligence(int intelligence) {
        this.intelligence = intelligence;
    }

    public void setHealthPoint(int healthPoint) {
        this.healthPoint = healthPoint;
    }
}
```

```java
Character character = new Character();
character.setStrength(20);
character.setDex(30);
character.setIntelligence(10);
character.setHealthPoint(0);
```

점층적 생성자 패턴의 단점들이 자바빈즈 패턴에서는 더 이상 보이지 않게됩니다. 코드가 길어지긴 했지만 인스턴스를 만들기 쉽고, 그 결과 더 읽기 쉬운 코드가 되었습니다. 하지만 단점을 지니고 있습니다.

- 자바빈즈 패턴에서는 필드에 값을 설정하기 위해 여러개의 setter 메소드를 호출
- 이로 인해 객체가 완전히 생성되기 전에 일부 값이 설정되어 일관성이 무너질 수 있습니다.

```java
Character character = new Character(); 

//일부 필드만 설정
character.setStrength(20);
character.setDex(30);

//character 객체를 사용하는 메소드들을 호출

//남은 필드를 setter를 통해 값 설정
character.setIntelligence(10);
character.setHealthPoint(0);
```

- `Character` 객체는 일부 필드만 설정되어 객체가 완전히 초기화되지 않은 상태로 사용될 수 있습니다.
- 객체는 일관성이 깨지고 예상치 못한 버그가 발생할 수 있습니다.

일관성이 무너지는 문제 떄문에 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며 쓰레드 안정성을 얻으려면 추가 작업을 해주어야 합니다.

> 왜 자바빈즈 패턴은 클래스를 불변으로 만들 수 없을까?

자바빈즈 패턴은 기본 생성자를 통해 객체를 생성하고 setter를 통해 필드를 변경하는데, 이렇게 객체의 상태를 변경할 경우 객체의 일관성이 중간에 깨질 수 있기 때문입니다. 따라서 `불변 클래스`로 만들어 불변하게 관리할 수 있을 것 같습니다.

```java
public final class ImmutableCharacter {

    private final int strength;
    private final int dex;
    private final int intelligence;
    private final int healthPoint;

    public ImmutableCharacter(int strength, int dex, int intelligence, int healthPoint) {
        this.strength = strength;
        this.dex = dex;
        this.intelligence = intelligence;
        this.healthPoint = healthPoint;
    }

    public int getStrength() {
        return strength;
    }

    public int getDex() {
        return dex;
    }

    public int getIntelligence() {
        return intelligence;
    }

    public int getHealthPoint() {
        return healthPoint;
    }
}
```

- 필드는 final로 선언하여 불변하게 만든다.
- setter 메소드를 구현하지 않는다.

불변 클래스 `ImmutableCharacter`는 필드를 수정할 수 없으므로 쓰레드 안전성이 보장됩니다.(별도의 동기화가 필요하지 않음)

## 빌더 패턴(Builder Pattern)

- 빌더 패턴은 점층적 생성자 패턴의 안전성과 자바빈 패턴의 가독성을 결합한 패턴
- 필요한 매개변수를 빌더 객체를 통해 설정하고, 선택적 매개변수는 필요한 만큼 설정

```java
public class Character {

    private final int strength;
    private final int dex;
    private final int intelligence;
    private final int healthPoint;

    private Character(Builder builder) {
        this.strength = builder.strength;
        this.dex = builder.dex;
        this.intelligence = builder.intelligence;
        this.healthPoint = builder.healthPoint;
    }

    public static class Builder {
        //필수 매개변수
        private final int strength;
        private final int dex;
        private final int intelligence;

        //선택 매개변수
        private int healthPoint = 0;

        public Builder(int strength, int dex, int intelligence) {
            this.strength = strength;
            this.dex = dex;
            this.intelligence = intelligence;
        }

        public Builder healthPoints(int healthPoint) {
            this.healthPoint = healthPoint;
            return this;
        }

        public Character build() {
            return new Character(this);
        }
    }
}
```

```java
Character character = new Character.Builder(20, 30, 10)
    .healthPoints(100)
    .build();
```

마지막으로 매개변수가 없는 build() 메소드를 호출해 불변 객체를 얻습니다.

- 필드는 final로 선언되어 있습니다. 한 번 초기화된 이후는 변경할 수 없습니다.
- `Character` 클래스의 생성자는 `Builder` 클래스를 통해서만 접근이 가능하고, `Builder` 클래스는 필수 매개변수를 받습니다.
- 선택적 매개변수인 `healPoint`는 `Builder` 클래스를 통해 설정되고
- `Builder` 클래스는 `Character` 객체를 생성할 때 필드의 상태를 고정시키기 때문에 변경할 수 없게됩니다.

```java
Character character = new Character.Builder(20, 30, 10)
    .build();
```

선택적 매개변수인 `healthPoint` 없이 `Character`를 생성하였습니다. 이렇게 하면 `healthPoint` 값이 기본값인 0으로 설정됩니다. 즉, 선택적 매개변수를 생략할 경우 해당 값은 기본값으로 초기화됩니다. 빌더 패턴은 매개변수에 따라 다양한 객체를 만들 수 있습니다. 그리고 빌더 패턴을 통해 유연하게 빌더 하나로 매번 생성하는 객체에 조금씩 변화를 줄 수 있습니다.(만드는 객체에 시리얼 번호를 증가하는 식으로)

단점으로는 객체를 만들기 전 빌더를 만들어야 하는데 성능에 민감한 상황에서는 문제가 될 수 있습니다. 그리고 생성자를 사용하는 것보다 코드가 길어집니다. 따라서 빌더 패턴은 매개변수가 많거나 앞으로 늘어날 가능성이 있는 경우에 사용하는 것이 좋습니다.

---

## private 생성자나 열거 타입으로 싱글톤임을 보증하라

`싱글톤`이란 인스턴스를 오직 하나만 생성할 수 있는 클래스로 `Character`를 싱글톤으로 만들면 다음과 같습니다.

**클래스를 싱글톤으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있습니다.** 싱글톤이 인터페이스를 구현한게 아니라면 mock으로 교체하기 어렵기 때문입니다.

## 싱글톤을 만드는 방식

싱글톤을 만드려면, 생성자는 `private`으로 감춰두고, 유일한 인스턴스에 접근할 수 있는 수단으로 `public static` 멤버를 하나 마련해 둡니다.

### 1. public static 멤버가 final 필드인 방식

```java
public class Character {

    public static final Character INSTANCE = new Character();

    private Character() {
    }
}
```

```java
Character character = Character.INSTANCE;
Character character2 = Character.INSTANCE;

System.out.println(character);   //Character@4617c264
System.out.println(character2);  //Character@4617c264
```

`private` 생성자는 `public static final` 필드를 초기화할 때 딱 한 번 호출됩니다. `public`이나 `protected` 생성자가 없으므로 `Character` 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나뿐임이 보장됩니다.

하지만 리플렉션 API인 `AccessibleObject.setAccessible`을 사용해 `private` 생성자를 호출할 수 있습니다.

아래 예시는 정적 팩토리 메소드를 사용해 싱글톤을 구현한 것입니다.

### 2. 정적 팩토리 메소드를 활용하는 방식

```java
public class Character {

    private static final Character INSTANCE = new Character();

    private Character() {
    }

    public static Character getInstance() {
        return INSTANCE;
    }
}
```

```java
Character character = Character.getInstance();
Character character2 = Character.getInstance();

System.out.println(character);   //Character@4617c264
System.out.println(character2);  //Character@4617c264
```

`getInstance()` 메소드는 항상 같은 객체를 반환하므로 인스턴스는 항상 동일한 것을 반환합니다.

정적 팩토리 메소드의 방식의 첫 번째 장점은 API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있습니다. 처음엔 싱글톤으로 쓰다가 나중엔 쓰레드당 새 인스턴스를 만드는 등 클라이언트 코드(`Character` 클래스를 사용하는 코드)를 고치지 않고도 변경할 수 있습니다.

```java
public class Character {

    private static final ThreadLocal<Character> instances = ThreadLocal.withInitial(Character::new);

    private Character() {
    }

    public static Character getInstance() {
        return instances.get();
    }
    
    public static Character getNewInstance() {
        return new Character();
    }
}
```

아래는 테스트 코드입니다.

```java
Character newInstance = Character.getNewInstance();
Character newInstance2 = Character.getNewInstance();
System.out.println("New Instance Name: " + newInstance);  //New Instance Name: Character@5ca881b5
System.out.println("New Instance2 Name: " + newInstance2);  //New Instance2 Name: Character@24d46ca6
```

### 3. 원소가 하나인 열거 타입을 선언

```java
public enum Character {

    INSTANCE; //원소가 하나인 열거 타입

    public String getName() {
        return "juseon";
    }
}
```

`Character`은 원소가 하나인 열거 타입으로, 싱글톤입니다. `enum`은 JVM에서 하나의 인스턴스만 존재하게 되기 때문에, 멀티쓰레드 환경에서도 안전하게 동작합니다.

```java
String name = Character.INSTANCE.getName();
```

`Character.INSTANCE`를 통해 항상 동일한 인스턴스에 접근하고 이를 통해 데이터를 공유할 수 있습니다. 또한, 이 방식은 직렬화에 대한 안전성을 제공해줍니다. 하지만 이 방법은 `enum` 말고 다른 상위 클래스를 상속해야 한다면 사용할 수 없습니다.(인터페이스는 구현 가능)
