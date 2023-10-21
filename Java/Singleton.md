`싱글톤`이란 인스턴스를 오직 하나만 생성할 수 있는 클래스로 `Character`를 싱글톤으로 만들면 다음과 같습니다.

- `Character` 클래스는 private 생성자를 가지며
- `getInstance()` 메소드를 통해 인스턴스를 생성하거나 반환합니다.

```java
public class Character {

    private static Character instance;
    
    private int healthPoints;
    private String name;
    
    private int strength;
    private int dex;      
    private int intelligence; 
    
    private Character() {
    }
    
    public static Character getInstance() {
        if (instance == null) {
            instance = new Character();
        }
        return instance;
    }
    
    public void initialize(String name, int healthPoints, int strength, int dex, int intelligence) {
        this.name = name;
        this.healthPoints = healthPoints;
        this.strength = strength;      
        this.dex = dex;              
        this.intelligence = intelligence; 
    }
    
    public void printInfo() {
        System.out.println("Character: " + name + ", HP: " + healthPoints);
        System.out.println("Strength: " + strength + ", Dexterity: " + dex + ", Intelligence: " + intelligence);
    }
}
```

클래스를 싱글톤으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있습니다.

1. 싱글톤 객체는 전역적으로 공유되기 때문에 하나의 테스트에서 상태를 변경하면 다른 테스트에 영향을 미칠 수 있습니다.
2. 만약, 테스트 중에 Character 객체의 동작을 변경하거나 
