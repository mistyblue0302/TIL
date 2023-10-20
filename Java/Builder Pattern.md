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

이 상황에서 healthPoint가 0인 경우를 나타내기 위해 int strength, dex, intelligence 세 가지의 매개변수만을 받는 생성자를 추가해야 합니다. 이렇게 선택 매개변수에 대한 모든 가능한 조합을 다루기 위해 다수의 오버로드된 생성자나 정적 팩토리 메소드가 필요해집니다.

선택적 매개변수가 많아지면 가독성을 해치고 코드를 작성하기 어려워집니다. 코드를 읽을 때 각 값의 의미가 무엇인지 헷갈릴 것이고, 매개변수의 개수 또한 확인해보아야 합니다. 만약, 타입이 같은 매개변수가 여러 개 있을 경우 찾기 어려운 버그로 이어질 수 있습니다. 클라이언트가 실수로 매개변수의 순서를 바꾸어도 컴파일러는 감지하지 못하고, 결국 런타임에 버그의 가능성이 높아집니다.

> 선택 매개변수가 많을 때 활용할 수 있는 대안이 있을까?

## 자바빈즈 패턴(JavaBeans Pattern)

- 매개변수가 없는 생성자로 객체를 만든 후, setter 메소드들을 호출해 원하는 매개변수의 값을 설정하는 방식

```java
public class Character {

    private int strength;
    private int dex;
    private int intelligence;
    private int healthPoint;

    public Character() {
        // 기본 생성자
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
- 이로 인해 객체가 완전히 생성되기 전에 일부 값이 설정되어 일관성이 무너질 수 있다.

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

- character 객체는 일부 필드만 설정되어 객체가 완전히 초기화되지 않은 상태로 사용될 수 있습니다.
- 객체는 일관성이 깨지고 예상치 못한 버그가 발생할 수 있습니다.

일관성이 무너지는 문제 떄문에 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며 스레드 안정성을 얻으려면 추가 작업을 해주어야 합니다.

> 왜 자바빈즈 패턴은 클래스를 불변으로 만들 수 없을까?

자바빈즈 패턴은 기본 생성자를 통해 객체를 생성하고, setter를 통해 필드를 변경하는데 이렇게 객체의 상태를 변경할 경우 객체의 일관성이 중간에 깨질 수 있기 때문입니다. 따라서 `불변 클래스`로 만들어 불변하게 관리할 수 있을 것 같습니다.

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
- Character 클래스의 생성자는 Builder 클래스를 통해서만 접근이 가능하고, Builder 클래스는 필수 매개변수를 받습니다.
- 선택적 매개변수인 healPoint는 Builder 클래스를 통해 설정되고
- Builder 클래스는 Character 객체를 생성할 때 필드의 상태를 고정시키기 때문에 변경할 수 없게됩니다.

빌더 패턴은 유연하며 매개변수에 따라 다양한 객체를 만들 수 있습니다.

```java
Character character = new Character.Builder(20, 30, 10)
    .build();
```

```java
Character warrior = new Character.Builder(30, 20, 10)
    .healthPoints(150)
    .build();
```



