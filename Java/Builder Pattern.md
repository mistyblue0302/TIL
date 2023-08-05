## 빌더 패턴 (Builder Pattern)

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
