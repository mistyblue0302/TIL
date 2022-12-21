## 기본 자료형(Primitive Type)

- 비객체 타입으로 null값을 가질 수 없다.
- 실제 값을 저장하며 Stack 메모리에 저장된다.

## 참조 자료형(Reference Type)

- Class, Interface, Array, Enum 타입이 있다.
- null값을 가질 수 있다.
- 실제 값은 Heap 영역에 저장하고 주소 값을 Stack 영역에 저장
- new로 인하여 생성하는 것들은 메모리 영역인 Heap 영역에 생성을 하게되고, GC가 수행되면서 메모리를 해제한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Immutable.png)

Java에서는 Primitive Type끼리만 산술 연산 및 논리 연산이 가능하다. 예외적으로 Reference Type은 `==` 연산자를 통해 가지고 있는 해시코드의 값으로 주소값을 비교할 수 있다. 값 비교가 필요할 경우 `Object.equals` 메소드를 오버라이딩하여 동등성 비교를 할 수 있다.
