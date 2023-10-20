`싱글톤`이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말합니다. 예시는 다음과 같습니다.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (Singleton.instance == null) {
            Singleton.instance = new Singleton();
        }

        return Singleton.instance;
    }
}
```
