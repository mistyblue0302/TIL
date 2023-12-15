## Serializable

- Serializable은 Java에서 객체를 직렬화할 수 있게 하는 인터페이스이다.
- **직렬**화란 **객체를 Byte 코드로 변환하는 프로세스**를 말하고, 이를 통해 객체를 파일에 저장하거나 네트워크를 통해 전송할 수 있다.
- 직렬화한다는 것은 Java 객체를 JVM에서 뽑아내서 데이터를 파일과 같은 형태로 저장할 수 있음을 의미한다.
- 이렇게 저장된 Byte를 다른 시스템에서 가져와서 사용할 수 있게된다.

대표적인 직렬화 포맷으로는 JSON, XML이 있다.

**transient** 예약어를 사용하여 선언한 변수는 Serializable의 대상에서 제외된다. 즉 해당 객체는 저장 대상이 되지 않는다.
보안상 중요한 변수나 꼭 저장해야 할 필요가 없는 변수에 대해 사용할 수 있다.

## NIO

NIO는 Java에서 제공하는 I/O 패키지 중 하나로, Non-blocking I/O를 지원한다. 채널과 버퍼를 사용한다.

- 채널
  - 입출력 작업을 수행하는 객체
- 버퍼
  - 임시로 데이터를 저장하는 공간

### NIO의 버퍼 클래스

각 데이터 타입(Primitive data types)에는 이와 대응하는 Buffer 클래스가 존재한다.

- ByteBuffer: 바이트 데이터를 저장하는 버퍼.
- CharBuffer: 문자 데이터를 저장하는 버퍼.
- ShortBuffer: short형 데이터를 저장하는 버퍼.
- IntBuffer: int형 데이터를 저장하는 버퍼.
- LongBuffer: long형 데이터를 저장하는 버퍼.
- FloatBuffer: float형 데이터를 저장하는 버퍼.
- DoubleBuffer: double형 데이터를 저장하는 버퍼.

### 각 버퍼의 주요 메소드
- capacity() : 버퍼가 포함할 수 있는 데이터의 최대 크기
- limit() : 버퍼에서 읽거나 쓸 수 없는 첫 위치
- position() : 현재 버퍼의 위치

<br>

참고

https://docs.oracle.com/javase/7/docs/api/java/io/Serializable.html

https://docs.oracle.com/javase/8/docs/api/java/nio/package-summary.html
