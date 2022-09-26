## Primitive Type vs Reference Type

### 기본 자료형(Primitive Type)

- 비객체 타입으로 null값을 가질 수 없다.
- 실제 값을 저장하며 Stack 메모리에 저장된다.

### 참조 자료형(Reference Type)

- Class, Interface, Array, Enum 타입이 있다.
- null값을 가질 수 있다.
- 실제 값이 아닌 객체의 주소값(해시코드값)을 저장한다.
- new로 인하여 생성하는 것들은 메모리 영역인 Heap 영역에 생성을 하게되고, Garbage Collector가 수행되면서 메모리를 해제한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Immutable.png)

Primitive Type은 그대로 Stack 영역에 저장이 되지만, Reference Type은 실제 값들은 Heap 영역에 저장하고 주소 값을 Stack 영역에 저장한다.
