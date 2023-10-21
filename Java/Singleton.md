`싱글톤`이란 인스턴스를 오직 하나만 생성할 수 있는 클래스로 `Character`를 싱글톤으로 만들면 다음과 같습니다.

- `Character` 클래스는 private 생성자를 가지기 때문에 클래스 외부에서 생성자를 호출하여 인스턴스를 만들지 못합니다.
- `getInstance()` 메소드를 통해 인스턴스를 생성하거나 반환합니다. `static`이기 때문에 해당 클래스가 로드될 때 한 번 초기화되고 프로그램 실행 동안 유지됩니다.

## 1. public static으로 인스턴스를 생성하는 방식

```java
public class Character {

    private static Character instance;
    private String name;

    private Character() {
    }

    public static Character getInstance() {
        if (instance == null) {
            instance = new Character();
        }
        return instance;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

클래스를 싱글톤으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있습니다. 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 만든 싱글턴이 아니라면 싱글턴 인스턴스를 가짜(mock) 구현으로 대체할 수 없기 때문입니다.

아래 예시는 정적 팩토리 메소드를 사용해 싱글톤을 구현한 것입니다.

## 2. 정적 팩토리 메소드를 활용하는 방식

```java
public class Character {

    private static Character instance;
    private String name;

    private Character() {
    }

    public static Character getInstance() {
        if (instance == null) {
            instance = new Character();
        }
        return instance;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

정적 팩토리 메소드의 방식의 첫 번째 장점은 API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있습니다. `getInstance()` 메소드가 항상 새로운 인스턴스를 생성하도록 변경합니다.

```java
public class Character {

    private String name;

    private Character() {
    }

    public static Character getInstance() {
        return new Character(); //항상 새로운 인스턴스 생성
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

## 3. 원소가 하나인 열거 타입을 선언

```java
public enum Character {

    INSTANCE; //원소가 하나인 열거 타입

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
`Character`은 원소가 하나인 열거 타입으로, 싱글톤입니다. enum은 JVM에서 하나의 인스턴스만 존재하게 되기 때문에, 멀티쓰레드 환경에서도 안전하게 동작합니다.

```java
Character character = Character.INSTANCE;
character.setName("LEE");

System.out.println(character.getName()); //LEE

Character anotherCharacter = Character.INSTANCE; //동일한 인스턴스
System.out.println(anotherCharacter.getName()); //LEE
```

`Character.INSTANCE`를 통해 항상 동일한 인스턴스에 접근하고 이를 통해 데이터를 공유할 수 있습니다.
