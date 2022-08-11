## JVM(Java Virtual Machine)이란?

자바 바이트 코드를 실행할 수 있는 주체로, 운영체제의 종류와 무관하게 자바 코드를 컴파일하여 얻은 바이트 코드를 기계어로 바꿔 실행시켜주는 역할을 한다.

> 바이트 코드? JVM이 알아들을 수 있는 명령어 집합

### JVM의 구조

먼저 자바 컴파일러에 의해 자바 소스파일(.java)은 바이트 코드(.class)로 변환되는데, 이러한 바이트 코드를 JVM에서 읽어들인다.

![img](https://media.vlpt.us/images/litien/post/a65da4a8-5dc4-422b-b91e-cafeafe464d3/image.png)

JVM의 구조는 Class Loader, Execution Engine, Garbage Collector, Runtime Data Area 4가지로 나눌 수 있다.

- Class Loader

생성된 .class파일에 들어있는 정보는 Class Loader에 의해 Runtime Data Area로 적재된다.

- Execution Engine

  Class Loader에 의해 메모리에 적재된 .class파일들을 기계어로 바꾸고 실행시킨다.

- Garbage Collector

  GC는 Heap 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 제거하는 역할을 한다.

- Runtime Data Area

  JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 곳으로 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있다.

  - Method Area 

    class에 대한 정보가 저장되는 영역

  - Heap 

    런타임 데이터, new 키워드로 생성된 객체나 배열이 생성되는 영역으로
    GC가 참조되지 않는 메모리를 확인하고 제거한다.

  - Stack

    메소드를 실행하기 위해 필요한 정보가 저장되는 영역으로 매개변수나 지역변수가 저장된다.

  - PC Register

    현재 실행되고 있는 명령의 주소를 저장하고 있는 영역

  - Native Method Stack

    자바 외 언어로 작성된 코드를 위한 메모리 영역


    쓰레드 생성을 기준으로 Method와 Heap영역은 모든 쓰레드가 공유하며 Stack, PC Register, Native Method Stack은 공유되지 않는다.









