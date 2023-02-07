## Collection

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Collection.png)

자바에서 컬렉션 프레임워크는 목록성 데이터를 처리하는 자료 구조를 통칭한다. List와 Set, Queue는 Collection 인터페이스를 구현하고, Collection 인터페이스는 다음과 같이 선언되어 있다.

```java
public interface Collection<E> extends Iterable<E>
```

Iterable 인터페이스에 선언되어 있는 메소드는 iterator() 하나로, Collection에 저장된 요소를 순차적으로 가져온다.

