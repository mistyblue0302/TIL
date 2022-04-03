# 💡 **Serializable과 NIO**


**transient**

객체를 저장하거나 다른 JVM으로 보낼 때, transient 예약어를 사용하여 선언한 변수는
Serializable의 대상에서 제외된다. 즉 해당 객체는 저장 대상이 되지 않는다.
보안상 중요한 변수나 꼭 저장해야 할 필요가 없는 변수에 대해 사용할 수 있다.

<br>

**NIO**

NIO는 속도 때문에 생겼으며, Stream 대신 채널과 버퍼를 사용한다.

**NIO의 버퍼 클래스**

각 데이터 타입(Primitive data types)에는 이와 대응하는 Buffer 클래스가 존재한다.

- capacity()는 버퍼가 포함할 수 있는 데이터의 최대 크기이다.

- limit()는 버퍼에서 읽거나 쓸 수 없는 첫 위치를 리턴한다.

- position()은 현재 버퍼의 위치를 리턴한다.

