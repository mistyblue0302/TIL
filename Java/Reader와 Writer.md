## Reader와 Writer

Reader와 Writer은 **char** 기반의 문자열을 처리하기 위한 클래스이다.

```java
public abstract class Reader
extends Object
implements Readable, Closeable
```

```java
public abstract class Writer
extends Object
implements Appendable, Closeable, Flushable
```

Appendable 인터페이스는 각종 문자열을 추가하기 위해 선언되었다.
Writer에 있는 write()나 append()메소드를 사용하여 데이터를 쓸 경우, 메소드를 호출할 때마다 파일에 쓰기 때문에 매우 비효율적이다. 이러한 단점을 보완하기 위한 BufferedWriter 클래스가 있다.
