# 💡 **JVM 메모리 구조**

먼저 자바 컴파일러에 의해 자바 소스파일은 바이트 코드(.class)로 변환된다. 그리고 이러한 바이트 코드를 JVM에서 읽어들인다.


![img](https://media.vlpt.us/images/litien/post/a65da4a8-5dc4-422b-b91e-cafeafe464d3/image.png)

<br>

**JVM(Java Virtual Machine)**

자바 바이트 코드를 실행할 수 있는 주체로 운영체제의 종류와 무관하게 자바 코드를 컴파일하여 얻은 바이트 코드를 기계어로 바꿔 실행시켜주는 역할을 한다.

<br>

 JVM의 구조는 Class Loader, Execution Engine, Garbage Collector, Runtime Data Area 4가지로 나눌 수 있다.

- Class Loader

  생성된 .class파일들을 Runtime Data Area로 적재한다.

- Execution Engine

  Class Loader에 의해 메모리에 적재된 .class파일들을 기계어로 바꾸고 실행하는 역할을 한다.

- Garbage Collector

  GC는 Heap 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 제거하는 역할을 한다.

- Runtime Data Area

  JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 곳으로 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있다.

  - Method Area 

    메소드 이름과 접근제어자 정보, 상수, static 변수 등이 생성되는 영역

  - Heap 

    new 키워드로 생성된 객체나 배열이 생성되는 영역 <br>
    GC가 참조되지 않는 메모리를 확인하고 제거하는 영역

  - Stack

    지역변수, 매개변수, 연산에 사용되는 임시 값 등이 생성되는 영역

  - PC Register

    쓰레드가 실행되는 주소와 명령을 저장하고 있는 영역

  - Native Method Stack

    자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역


    쓰레드 생성을 기준으로 Method와 Heap영역은 모든 쓰레드가 공유하며 Stack, PC Register, Native Method Stack은 공유되지 않는다.









