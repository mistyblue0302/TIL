## 정적 팩토리 메소드(Static Factory Method) 

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

만약 생성자를 사용해 캐릭터를 만들어 준다면, 다음과 같을 것입니다. 하지만 new 키워드를 통해 생성하면, 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 캐릭터 특성을 제대로 설명하지 못합니다. 변수명으로 구분지어 주지 않는다면 연속된 숫자만으로 캐릭터의 직업을 유추하기 어렵습니다.

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

    private int value;

    private Character(int value) {
        this.value = value;
    }

    public static Character defaultCount() {
        return new Character(0);
    }

    public static Character createCount(int value) {
        return new Character(value);
    }
}
```

위의 코드에서 생성자는 int value 값을 받는 유일한 시그니처를 가지고 있습니다. 하지만 `defaultCount()`와 `createCount`라는 두 개의 정적 팩토리 메소드를 사용하면 두 가지 방법으로 객체를 생성할 수 있습니다.

```java
Character defaultCharacter = Character.defaultCount(); //0
Character customCharacter = Character.createCount(32); //32
```

첫 번째 방법은 `생성자를 이용해 value를 0으로 초기화한` Character 객체를 생성하고 값을 초기화 합니다. 두 번째 방법은 `매개변수 값으로 value를 초기화한` Character 객체를 생성합니다.

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

new 키워드를 사용하면 인스턴스가 그때 그때 생성되는 반면, 정적 팩토리 메소드는 static을 통해 미리 생성된 Character 객체를 반환하기 때문에 매번 새로운 인스턴스를 생성하지 않아도 됩니다. 따라서 생성 비용이 큰 객체가 자주 요청되는 상황이라면 성능이 향상될 수 있습니다.

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

정적 팩토리 메소드를 사용하면 메소드 호출을 통해 얻을 객체의 인스턴스를 선택 가능하게 해줍니다.(다형성) Wizard와 Warrior 클래스는 Character 클래스의 하위 클래스로 Character 클래스에 정의된 makeCharacter() 메소드를 호출하면 Character 타입의 객체를 반환합니다. 이것은 다형성을 활용한 것으로, 정적 팩토리 메소드를 통해 해당 하위 클래스의 객체를 얻을 수 있습니다.

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

- Character 클래스의 정적 팩토리 메소드를 호출하여 Character 객체를 생성하고 초기화 합니다.
- Wizard.makeWizard()를 호출하면 Wizard 클래스의 정적 팩토리 메소드가 실행되고 이 메소드는 상위 클래스인 Character의 생성자를 호출하여 초기화 합니다.
- Warrior.makeWarrior()은 2번과 같은 원리로 동작하고 상위 클래스인 Character의 생성자를 호출하여 초기화 합니다.

정적 팩토리 메소드를 통해 객체 생성에 관한 로직을 숨길 수 있습니다. 클라이언트 코드(main)는 구체적인 객체 생성 방법(makeCharacter())을 알 필요가 없고, 클래스 내부가 변경되어도 영향을 받지 않습니다. 단순히 makeWizard() 또는 makeWarrior() 메소드를 호출하여 객체를 얻을 수 있습니다.

```java
public class Rogue extends Character {

    public Rogue(int strength, int dex, int intelligence) {
        super(strength, dex, intelligence);
    }

    public static Rogue makeRogue(int dex) {
        return new Rogue(5, dex, 20);
    }
}
```

또한 반환되는 객체를 다양하게 선택할 수 있습니다. 새로운 하위 클래스를 추가하거나 기존 클래스에 변경이 생길 시 기존의 정적 팩토리 메소드만 수정하여 새로운 객체를 반환할 수 있습니다.


### 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.


### Lombok의 RequiredArgsConstructor 

Lombok의 `RequiredArgsConstructor`를 사용하면 정적 팩토리 메소드를 쉽게 만들 수 있다.

```java
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor(staticName = "of")
public class User {
    private final Long id;
    private final String name;
}
```

위와 같이 staticName을 사용하면 lombok이 of라는 이름을 가진 정적 팩토리 메소드를 만들어준다. 즉, 다음과 같이 사용할 수 있다.

```java
User user = User.of(1L, "JuSeon");
```





[https://luckydavekim.github.io/development/etc/effective-java/item1-consider-static-factory-methods-instead-of-constructors](https://luckydavekim.github.io/development/etc/effective-java/item1-consider-static-factory-methods-instead-of-constructors)
