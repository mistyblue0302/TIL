`싱글톤`이란 인스턴스를 오직 하나만 생성할 수 있는 클래스로 `Character`를 싱글톤으로 만들면 다음과 같습니다.

**클래스를 싱글톤으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있습니다.** 싱글톤이 인터페이스를 구현한게 아니라면 mock으로 교체하기 어렵기 때문입니다.

## 1. public static 멤버가 final 필드인 방식

```java
public class Character {

    private static final Character instance = new Character();
    
    private Character() {
    }
}
```

- `Character` 클래스는 private 생성자를 가지기 때문에 클래스 외부에서 생성자를 호출하여 인스턴스를 만들지 못합니다.(유일함 보장)
- `static final`이기 때문에 해당 클래스가 로드될 때 한 번 초기화되고 프로그램 실행 동안 유지됩니다.

하지만 클라이언트는 리플렉션 API를 사용해 private 생성자를 호출할 수 있습니다. 

아래 예시는 정적 팩토리 메소드를 사용해 싱글톤을 구현한 것입니다.

## 2. 정적 팩토리 메소드를 활용하는 방식

```java
public class Character {

    private static final Character instance = new Character();

    private Character() {
    }

    public static Character getInstance() {
        return instance;
    }
}
```

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

아래는 테스트 클라이언트 코드입니다.

```java
//기존의 싱글톤 인스턴스
Character singletonInstance = Character.getInstance();  
Character singletonInstance2 = Character.getInstance();  
System.out.println("Singleton Instance Name: " + singletonInstance);  //Singleton Instance Name: Character@5ca881b5
System.out.println("Singleton Instance2 Name: " + singletonInstance2);  //Singleton Instance2 Name: Character@5ca881b5

//변경된 싱글톤 인스턴스
Character newInstance = Character.getNewInstance();
Character newInstance2 = Character.getNewInstance();
System.out.println("New Instance Name: " + newInstance);  //New Instance Name: Character@24d46ca6
System.out.println("New Instance2 Name: " + newInstance2);  //New Instance2 Name: Character@4517d9a3
```

## 3. 원소가 하나인 열거 타입을 선언

```java
public enum Character {

    INSTANCE; //원소가 하나인 열거 타입

    public String getName() {
        return "juseon";
    }
}
```

`Character`은 원소가 하나인 열거 타입으로, 싱글톤입니다. enum은 JVM에서 하나의 인스턴스만 존재하게 되기 때문에, 멀티쓰레드 환경에서도 안전하게 동작합니다.

```java
String name = Character.INSTANCE.getName();
```

`Character.INSTANCE`를 통해 항상 동일한 인스턴스에 접근하고 이를 통해 데이터를 공유할 수 있습니다. 또한, 이 방식은 직렬화에 대한 안전성을 제공해줍니다. 하지만 이 방법은 enum 말고 다른 상위 클래스를 상속해야 한다면 사용할 수 없습니다.(인터페이스는 구현 가능)
