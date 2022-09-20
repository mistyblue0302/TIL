## JVM(Java Virtual Machine)이란?

자바 바이트 코드를 실행할 수 있는 주체로, 운영체제의 종류와 무관하게 자바 코드를 컴파일하여 얻은 바이트 코드를 기계어로 바꿔 실행시켜주는 역할을 한다.

> 바이트 코드란? JVM이 알아들을 수 있는 명령어 집합

### JVM의 구조

먼저 자바 컴파일러에 의해 자바 소스파일(.java)은 바이트 코드(.class)로 변환되는데, 이러한 바이트 코드를 JVM의 Class Loader에서 읽어들인다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/JVM.png)

- **Class Loader** : 바이트 코드를 읽고 .class파일에 들어있는 정보를 Heap/Method Area에 저장한다.

- **Execution Engine** : Class Loader에 의해 메모리에 적재된 바이트 코드(.class)를 기계어로 바꾸고 실행시킨다.

- **Garbage Collector** : GC는 Heap 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 제거하는 역할을 한다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Runtime%20Data%20Area.png)

- **Runtime Data Area** : JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 곳이다.
  - **Method Area** : class에 대한 정보가 저장되는 영역으로, 메소드의 코드를 저장한다.
   - **Heap** : 런타임에 생성된 모든 객체 인스턴스들이 저장되는 영역이다. new 키워드로 생성된 객체나 배열이 생성되는 영역으로 GC가 참조되지 않는 메모리를 확인하고 제거한다.
   - **Stack** : 메소드 호출을 스택 Frame이라는 블록으로 쌓으며 로컬 변수, 매개변수, 중간 연산 결과들이 저장된다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Stack%20Frame.png)

메소드를 실행하기 위해 필요한 정보가 저장되는 영역으로 Stack은 Frame이라는 자료구조로 저장이 되는데, 메소드가 호출될 때마다 생성되며 메소드가 끝나거나 예외 발생 시 소멸된다.

Stack의 Frame은 다음과 같이 세 개의 부분으로 구성되어 있다.

   1. **Local Variable Array**

메소드 내의 지역 변수들의 값을 배열로 담아놓고 인덱스로 접근이 가능하다. 배열의 첫 인덱스는 현재 인스턴스의 참조 this를 가지고 있다.
 
   2. **Operand Stack**

   연산을 위한 작업 공간으로 연산의 중간값을 저장한다.

   3. **Constant Pool Reference**

   클래스 내에 사용되는 상수들을 담는 영역으로, index와 type 그리고 매핑된 값으로 되어있다. JVM은 Constant Pool을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조한다.

- **PC Register** : 현재 실행되고 있는 스택 Frame의 주소를 저장하고 있는 영역이다. 멀티 쓰레드 프로그래밍 환경에서 한 쓰레드가 작업을 하다가 다른 쓰레드로 잠시 cpu 점유를 넘겨주고 돌아왔을 때 이전에 어떤 명령을 했는지 기억해야 한다. 

- **Native Method Stack** : 자바 외 언어로 작성된 코드를 위한 메모리 영역

<br>

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/Run-time%20Data%20Area1.png)

Method Area와 Heap영역은 모든 쓰레드가 공유하는 곳으로, 멀티 쓰레드 프로그래밍을 할 때 동기화에 주의해야 하는 영역이다.
Stack, PC Register, Native Method Stack 영역은 쓰레드가 생성될 때마다 같이 생성되고 쓰레드 간에 공유되지 않는다.

<br>

참고

https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html(https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html)



