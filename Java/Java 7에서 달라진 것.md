## 💡 **Java 7에서 달라진 것**

### Java 7 변경사항

#### 1. 숫자 표시 방법

Java 7부터는 2진수 표현이 추가되었다. 2진수로 나타내기 위해서는 0b를 앞에 추가해 주면 된다. 또한 _를 사용하여 자리 단위를 나눌 수 있다. 반드시 숫자 사이에 넣어주어야 한다.

#### 2. switch문에서 String사용

switch-case에서 String 문자열을 바로 사용할 수 있다.

#### 3. 제네릭을 쉽게 사용하기 위한 Diamond

생성자에서도 해당 타입들을 상세하게 명시했어야 했지만 Java 7부터는 명시해 줄 필요가 없다. 컴파일러에서 알아서 해당 생성자의 타입을 지정해버린다.

```java
Map<String, Integer> map = new HashMap<String, Integer>();
Map<String, Integer> map = new HashMap<>();
```

#### 4. non reifiable varargs 타입

- reifiable : 실행 시에도 타입 정보가 남아있는 타입
- non reifiable : 컴파일시 타입 정보가 손실되는 타입

자바 제네릭 사용 중 발생 가능한 문제 중 하나가 reifiable 하지 않은 varargs 타입이다. 이 문제는 제네릭을 사용하지 않는 버전과의 호환성 때문에 발생한다.

#### 5. 예외 처리 시 다중 처리 가능

- 각 예외들을 | 로 연결하여 간단히 처리할 수 있다.

- try-with-resource : try의 소괄호에 close()를 이용해 닫아야 할 필요가 있는 객체를 적어준다. Java 7에는 AutoCloseable이라는 인터페이스가 추가되어 객체는 finally 문장에서 별도로 close()를 처리할 필요가 없다.

### Java 7 추가된사항

#### 1. Fork/Join : 여러 개로 나누어 계산한 후 결과를 모으는 작업

- Fork : 여러 개로 나누는 것
- Join : 나누어서 작업한 결과를 모으는 것
- Work Stealing : 여러 개의 Deque에 작업이 나뉘어져 진행될 때 한가한 Deque가 바쁜 Deque에 대기하고 있는 일을 해 주는 것

**사용 방법**

RecursiveAction/RecursiveTask를 상속 후 ForkJoinPool 클래스를 이용하여 작업을 시작

```java
public abstract class ResursiveAction extends ForkJoinTask<Void>
```

```java
public abstract class RecursiveTask<V> extends ForkJoinTask<V>
```

- ForkJoinTask 클래스

Future 인터페이스는 비동기적인 요청을 하고 응답을 기다릴 때 사용한다.

```java
public abstract class ForkJoinTask<V> extends Object implements Future<V>, Serializable
```

#### 2. NIO 2

- NIO(New I/O) : 데이터를 빠르게 읽고 쓰기 위함
- 기존 File 클래스는 여러 단점들이 존재

- File 클래스를 대체하는 클래스

Paths : get() 메소드를 사용하면 Path 인터페이스의 객체를 얻을 수 있으며 Path 인터페이스는 파일,경로 정보를 가진다.

Files : 기존 File 클래스 단점을 보완한 클래스로 Path 객체를 사용하여 파일을 통제한다.

FileSystems : 현재 사용중인 파일 시스템에 대한 정보를 처리하는 데 필요한 메소드를 제공하며 getDefault() 메소드를 사용하면 사용중인 기본 파일 시스템에 대한 정보를 갖고 있는 FileSystem 인터페이스와 객체 얻을 수 있다.

FileStore : 파일을 저장하는 디바이스, 파티션, 볼륨 등에 대한 정보들을 확인하는 데 필요한 메소드 제공한다.

- SeekableByteChannel(random access)

    채널은 디바이스, 파일, 네트워크 등과의 연걸 상태를 나타내는 클래스로 파일을 읽거나 네트워크에서 데이터를 받는 작업을 처리하기 위한 통로다. 바이트 기반의 채널을 처리하는 데 사용되며 채널을 보다 유연하게 처리 가능하다.

- NetworkChannel 및 MulticastChannel

    NetworkChannel은 네트워크 소켓을 처리하기 위한 채널로 네트워크 연결에 대한 바인딩, 소켓 옵션을 셋팅하고 로컬 주소를 알려주는 인터페이스이다. MulticastChnnel은 IP 멀티캐스트를 지원하는 네트워크 채널이다.(멀티 캐스트 = IP를 그룹으로 묶고, 그 그룹에 데이터를 전송하는 방식)

- AsynchronousFileChannel 및 AsynchronousChannelGroup

    Asynchronous는 비동기 처리를 의미한다. 비동기란 다른 작업이 끝나지 않아도 실행할 수 있는 것을 의미하는데, AsynchronousChannelGroup은 비동기적인 처리를 하는 쓰레드 풀을 제공하여 안정적인 비동기 처리가 가능하다.
