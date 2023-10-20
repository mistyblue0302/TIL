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

  

## 빌더 패턴(Builder Pattern)

빌더 패턴은 선택적 매개변수가 많은 상황에서 생성자 혹은 정적 팩토리 메소드보다 유용하게 사용할 수 있다.

햄버거를 주문할 때, 크기는 필수로 입력해야하지만 나머지 재료들은 주문하는 사람이 마음대로 결정할 수 있다고 하자. 여기서 필수 매개변수는 햄버거의 사이즈가 될 것이고, 여러 재료들은 선택 매개변수가 된다.

## 빌더 패턴을 적용하기 전

### 점층적 생성자 패턴 (Telescoping Constructor Pattern)

점층적 생성자 패턴은 필수 매개변수와 함께 선택 매개변수를 0개, 1개, 2개... 를 받는 형태로 선택 매개변수를 전부 다 받는 생성자까지 늘려가는 방식이다. 즉, 선택 매개변수가 전달되는 여러 경우의 수에 대해 생성자를 여러개 정의해야한다. 

```java
public class Hamburger {
    // 필수 매개변수
    private final int size;

    // 선택 매개변수
    private final int bun;
    private final int patty;
    private final int lettuce;
    private final int tomato;

    public Hamburger(int size, int bun, int patty, int lettuce, int tomato) {
        this.size = size;
        this.bun = bun;
        this.patty = patty;
        this.lettuce = lettuce;
        this.tomato = tomato;
    }

    public Hamburger(int size, int bun, int patty, int lettuce) {
        this(size, bun, patty, lettuce, 0);
    }

    public Hamburger(int size, int bun, int patty) {
        this(size, bun, patty, 0);
    }

    public Hamburger(int size, int bun) {
        this(size, bun, 0);
    }

    public Hamburger(int size) {
        this(size, 0);
    }
}
```

위 클래스를 사용하여 햄버거를 만드는 코드는 다음과 같다.

```java
//모든 재료가 있는 햄버거
Hamburger hamburger1 = new Hamburger(10, 2, 3, 6, 1);

//양상추와 토마토가 없는 햄버거
Hamburger hamburger2 = new Hamburger(10, 2, 5);

//빵과 토마토만 있는 햄버거
Hamburger hamburger3 = new Hamburger(10, 2, 0, 0, 6);
```

`hamburger3`는 `patty`와 `lettuce`가 필요 없다. 하지만 이를 생략한 생성자는 구현되어 있지 않으므로 직접 파라미터로 `0`을 전달해주어야 한다.

즉, 점층적 생성자 패턴은 **사용자가 설정하길 원치 않는 매개변수까지 포함되기 쉬운 형태**이다. 또한, **매개변수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다.** 따라서 잘못된 순서로 매개변수를 전달할 수 있다. 

### 자바빈즈 패턴 (JavaBeans Pattern)

자바빈즈 패턴은 선택적 매개변수가 많을 때 활용할 수 있는 두번째 대안이다. 매개변수가 없는 생성자로 객체를 만들고 setter 메소드들을 호출해 원하는 매개변수의 값을 설정하는 방식이다.

```java
public class Hamburger {
    private int size = -1; // 필수 매개변수
    private int bun = 0;
    private int patty = 0;
    private int lettuce = 0;
    private int tomato = 0;

    public Hamburger() {}

    public void setSize(int size) {
        this.size = size;
    }

    public void setBun(int bun) {
        this.bun = bun;
    }

    public void setPatty(int patty) {
        this.patty = patty;
    }

    public void setLettuce(int lettuce) {
        this.lettuce = lettuce;
    }

    public void setTomato(int tomato) {
        this.tomato = tomato;
    }
}
```

위 클래스로 점층적 생성자 패턴에서 생성했던 '빵과 토마토만 있는 햄버거'를 생성해보자.

```java
Hamburger hamburger = new Hamburger();
hamburger.setSize(3);
hamburger.setBun(2);
hamburger.setTomato(1);
```

더 이상 점층적 생성자 패턴의 단점이 보이지 않는다. 코드가 길어지긴 했지만 인스턴스를 만들기 쉽고, 더 읽기 쉬운 코드가 되었다.

**일관성 문제**

하지만, 자바빈즈 패턴도 자신만의 심각한 단점을 지니고 있다. 일단 객체를 생성하기 위해 여러개의 메소드를 호출해야한다. 또한 객체가 완전히 생성되기 전까지는 일관성이 무너진 상태에 놓이게 된다.

객체가 완전히 생성된 상태는 필수 매개변수가 모두 설정된 상태이다. 하지만 개발자의 실수로 `setSize()` 메소드를 사용하지 않은 상태라면, 그 객체는 유효하다고 말할 수 없다. 하지만, 일관성이 깨진 상태를 객체 생성 코드를 작성할 때나 컴파일 타임에는 알 수 없다.

일관성이 깨진 햄버거 인스턴스가 만들어지면 그 인스턴스를 사용하는 코드에서 런타임에 문제가 발생하게 될 것이다. 하지만, 이미 객체를 생성하는 부분과 문제가 발생한 코드가 물리적으로 멀리 떨어져 있으므로 디버깅이 쉽지 않다.

**불변성 문제**

또한 완전히 생성된 객체라고 하더라도 외부적으로 setter 메소드를 노출하고 있으므로, 불변함을 보장할 수 없게된다. 즉, 자바빈즈 패턴을 사용하면 클래스를 불변으로 만들 수 없다.

## 빌더 패턴을 사용하게되면

빌더 패턴은 빌더라는 객체를 통해 객체를 생성하게 된다. 사용자는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻는다. 그런 다음 빌더 객체가 제공하는 일종의 setter 메소드들로 원하는 선택 매개변수들을 설정한다. 빌더 객체의 setter 메소드의 반환값은 빌더 자기 자신이므로 메소드를 연결하여 연속으로 사용하는 메소드 체이닝을 할 수 있다.

```java
public class Hamburger {
    private final int size;
    private final int bun;
    private final int patty;
    private final int lettuce;
    private final int tomato;

    private Hamburger(Builder builder) {
        this.size = builder.size;
        this.bun = builder.bun;
        this.patty = builder.patty;
        this.lettuce = builder.lettuce;
        this.tomato = builder.tomato;
    }

    public static class Builder {
        // 필수 매개변수
        private final int size;

        // 선택 매개변수
        private int bun = 0;
        private int patty = 0;
        private int lettuce = 0;
        private int tomato = 0;

        public Builder(int size) {
            this.size = size;
        }

        public Builder bun(int bun) {
            this.bun = bun;
            return this;
        }

        public Builder patty(int patty) {
            this.patty = patty;
            return this;
        }

        public Builder lettuce(int lettuce) {
            this.lettuce = lettuce;
            return this;
        }

        public Builder tomato(int tomato) {
            this.tomato = tomato;
            return this;
        }

        public Hamburger build() {
            return new Hamburger(this);
        }
    }
}
```

Hamburger 클래스 내부에 static 클래스인 Builder가 정의되어 있다. Builder 클래스는 필수 매개변수인 size만을 전달받아 생성된다. 그리고 bun, patty, lettuce, tomato 와 같은 메소드는 빌더 자기자신의 매개변수를 설정하고 반환하는 것을 확인할 수 있다.

```java
Hamburger hamburger = new Hamburger.Builder(10)
    .bun(2)
    .lettuce(3)
    .patty(1)
    .build();
```

- 장점
    - 클라이언트 코드가 읽고 쓰기 쉽다.
    - 내부 빌더 객체가 생성자를 통해 객체를 생성하므로 불변 객체를 만들 수 있다.
    - 원하는 매개변수만 설정할 수 있다.
    - 어떤 값이 들어가는지 명시적으로 확인할 수 있다.
- 단점
    - 객체 내부에 빌더 클래스를 생성해야 하므로 코드가 길어지고 복잡해 진다.

매개변수가 4개보다 적다면, 점층적 생성자 패턴을 사용하는 것이 더 좋다고 한다. 빌더 패턴의 코드가 다소 장황하기 때문이다. 하지만, API는 시간이 지날수록 많은 매개변수를 가지는 경향이 있기 때문에 애초에 빌더 패턴으로 시작하는 편이 나을 때가 많다고 한다.

