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
