## 상속 vs 조합

상속이란,

- 기존에 정의되어 있는 클래스의 필드와 메소드를 물려받아 새로운 클래스를 생성하는 것으로
- 중복코드 제거와 기능확장을 할 수 있다.
- 클래스들의 계층적인 구조를 만들 수 있다.

### 상속의 문제

1. **하위 클래스가 상위 클래스의 구현에 의존하기 때문에 상위 클래스의 변경에 모든 하위 클래스가 영향을 받는다.**

로또 번호를 `List<Integer>`로 가지고 있는 역할인 `Lotto` 클래스가 있다. 

```java
public class Lotto {
  
    protected List<Integer> lottoNumbers;

    public Lotto(List<Integer> lottoNumbers) {
        this.lottoNumbers = new ArrayList<>(lottoNumbers);
    }

    public boolean contains(Integer integer) {
        return this.lottoNumbers.contains(integer);
    }
    ...
}
```
  
`Lotto` 클래스를 상속하는 `WinningLotto` 클래스는 당첨 로또번호를 가지고 있다.
  
```java
public class WinningLotto extends Lotto {
  
    private final BonusBall bonusBall;

    public WinningLotto(List<Integer> lottoNumbers, BonusBall bonusBall) {
        super(lottoNumbers);
        this.bonusBall = bonusBall;
    }

    public long compare(Lotto lotto) {
        return lottoNumbers.stream()
            .filter(lotto::contains)
            .count();
    }
    ...
}
```
 
하지만 `Lotto` 클래스의 요구사항이 바뀌어서 인스턴스 변수인 `List<Integer> lottoNumbers`가 `int[] lottoNumbers`로 바뀌었다고 가정해보자.

```java
public class Lotto {
  
    protected int[] lottoNumbers;

    public Lotto(int[] lottoNumbers) {
        this.lottoNumbers = lottoNumbers;
    }

    public boolean contains(Integer integer) {
        return Arrays.stream(lottoNumbers)
            .anyMatch(lottoNumber -> Objects.equals(lottoNumber, integer));
    }
    ...
}
```
  
부모와 강한 의존을 맺은 `WinningLotto` 클래스 역시 영향을 받는다.
  
```java
public class WinningLotto extends Lotto {
  
    private final BonusBall bonusBall;

    // 오류가 발생한다.
    public WinningLotto(List<Integer> lottoNumbers, BonusBall bonusBall) {
        super(lottoNumbers);
        this.bonusBall = bonusBall;
    }

    // 오류가 발생한다.
    public long compare(Lotto lotto) {
        return lottoNumbers.stream()
            .filter(lotto::contains)
            .count();
    }
}
```
  
즉, `Lotto` 클래스를 상속한 하위 클래스가 몇 개가 있든 상위 클래스의 변화로 인해 하위 클래스를 변경해주어야 한다. 이처럼 상속은 하위 클래스가 상위 클래스에 강하게 의존, 결합하기 때문에 변화에 유연하게 대처하기 어려워진다.
  
  
2. **상위 클래스의 public 메소드가 하위 클래스에도 노출된다.**

상속은 부모 클래스와 강하게 의존하기 때문에 부모 클래스의 캡슐화를 해치고 결합도가 높아진다. 부모 클래스의 구현을 변경하면, 많은 자식 클래스를 모두 변경 해줘야 하기 때문이다. 불필요한 메소드도 상속받는 문제가 있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Stack.png)

Stack 클래스는 Vector 클래스를 상속받고 있다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/StackTest.png) 

문자열을 저장하는 Stack을 선언 후 Stack에서 지원하는 메소드로 문자열을 넣어주었다. Stack에서 문자열을 꺼내면 마지막에 넣은 문자열이 반환될 것이라고 예상할 수 있다. 하지만 실제 실행 결과는 예상과 다르게 두 번째에 넣은 문자열이 반환된다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/StackTest2.png)

add() 메소드는 Stack의 규칙을 따르지 않기 때문이다. 원래 Stack은 나중에 들어온 원소부터 먼저 반환되어야 하는데 add() 메소드는 순서와 상관없이 특정 인덱스에 원소를 추가할 수 있게 허용하고 있다. Vector를 상속한 Stack은 자신에게 필요하지 않은 메소드를 노출할 수 밖에 없다.

### 조합은 상속의 문제점을 어떻게 해결할까?

조합은 private 필드로 기존 클래스의 인스턴스를 참조하고, 인스턴스의 메소드를 호출하는 방식으로 구현한다.
  
위에서 보았던 `WinningLotto` 클래스가 `Lotto`를 상속하는 것이 아닌 조합(Composition)을 사용하면 다음과 같다.

```java
public class WinningLotto {

    private Lotto lotto;
    private BonusBall bonusBall;
}  
```

`WinningLotto` 클래스에서 인스턴스 변수로 `Lotto` 클래스를 가지는 것이 조합(Composition)이다. `WinningLotto` 클래스는 `Lotto` 클래스의 메서드를 호출하는 방식으로 동작하게 된다.  
  
조합을 사용하면,

- 메소드를 호출하는 방식으로 동작하기 때문에 캡슐화를 깨뜨리지 않는다.
- Lotto 클래스 같은 기존 클래스의 변화에 영향이 적어지며 안전하다.
  
메소드 호출 방식이기 때문이 `Lotto` 클래스의 인스턴스 변수인 `List<Integer> lottoNumbers`가 `int[] lottoNumbers`로 바뀌어도 영향을 받지 않게 된다. 그저 메서드 호출을 통한 값을 사용하면 될 뿐이다.
  
### 상속과 조합은 언제 써야할까?

**상속의 목적**

1. 서브타이핑 - 다형적인 계층구조 구현(부모와 자식 행동이 호환)
2. 서브클래싱 - 다른 클래스의 코드를 재사용(부모와 자식 행동이 호환 x)

이 두가지의 서로 다른 **상속**의 차이는 부모 클래스의 행동과 자식 클래스의 행동의 호환여부이다. 두 객체가 서로 **Is-A** 관계이거나 클라이언트 관점에서 두 객체가 동일한 행동을 할 것이라 기대될 때 상속을 고려한다.

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/Inheritance.png)
  
위처럼 Is-A 관계일 때 상속을 고려한다. 포유류가 동물이라는 사실은 변할 가능성이 거의 없고, 포유류가 숨을쉬고 새끼를 낳는다는 행동 역시 변할 가능성은 거의 없다. 

```java
public class 포유류 extends 동물 {

    protected void 숨을쉬다() {
        ...
    }

    protected void 새끼를낳다() {
        ...
    }
}
```
  
반면 **조합**은 **Has-A** 관계이다. 객체가 변경되더라도 영향을 최소화할 수 있기 때문에 변경에 안정적이며 느슨하게 결합되므로 설계가 유연해진다.

