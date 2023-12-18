## InputStream과 OutputStream

자바의 I/O는 기본적으로 InputStream과 OutputStream이라는 abstract 클래스를 통해 제공된다. 데이터를 읽을 때에는 InputStream의 자식 클래스를 통해 읽고, 데이터를 쓸 때에는 OutputStream의 자식 클래스를 통해 쓴다. InputStream과 OutputStream은 byte를 다루기 위한 클래스이다.

### InputStream

```java
public abstract class InputStream
extends Object
implements Closeable
```

InputStream클래스는 Closeable 인터페이스를 구현했으며, 이 인터페이스에는 close()라는 메소드만 선언되어 있다. **스트림에서 작업중인 대상을 닫고 리소스를 해제**하는 것을 의미하고 close() 메소드를 호출하여 열었던 리소스를 닫아 주어야 한다.

데이터를 읽을 때는 read() 메소드를 사용하며 유일한 abstract 메소드다.

### OutputStream

```java
public abstract class OutputStream
extends Object
implements Closeable, Flushable
```

OutputStream의 메소드로는 write(), flush(), close()가 있다.
write()의 경우 매개변수로 받은 바이트를 저장하며, flush()는 Flushable 인터페이스에 선언되어 있는 메소드로 버퍼에 대기하고 있는 데이터를 강제로 쓰도록 한다.
