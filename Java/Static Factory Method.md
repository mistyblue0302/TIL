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
public class Wizard extends  Character{

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

정적 팩토리 메소드를 작성하는 시점에는 반환될 객체의 클래스가 무엇인지 알지 못합니다. 런타임 시 동적으로 객체의 클래스를 결정하고 생성할 수 있습니다.

> 장점만 있을까?

### 1. 정적 팩토리 메소드만 제공하면, 생성자가 없으므로 하위 클래스를 만들 수 없다. 

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
