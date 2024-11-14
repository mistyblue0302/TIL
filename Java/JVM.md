## JVM(Java Virtual Machine)이란?

Java 소스 코드를 컴파일하여 얻은 바이트 코드를 운영체제의 종류와 무관하게 실행시켜주는 역할을 한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/JVM2.png)

우선 **컴파일된 바이트코드를 JVM의 Class Loader에게 전달하면 Class Loader는 로딩(클래스 정보를 method area에 저장) 및 링크(클래스 검증, 메모리 할당)를 통해 각 Runtime Data Area에 할당한다.**

이후 **JVM의 실행 엔진을 통해 메모리 영역에 있는 클래스 인스턴스를 생성 후 메소드를 호출해 프로그램을 실행한다.**

## JVM의 구조

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/JVM.png)

- **Class Loader** : 런타임 중에 Method Area에 동적으로 Java 클래스를 로드하는 역할을 한다. 
  - Loading : 바이트 코드(.class)를 읽고 클래스 정보를 Method Area에 저장한다.
  - Linking : 읽어들인 클래스의 검증, 메모리 할당 등의 작업을 수행한다.
  - Initialization : 클래스 변수를 적절한 값으로 초기화한다. 즉, static 필드들이 설정된 값으로 초기화된다.

- **Runtime Data Area** : JVM이 운영체제 위에서 실행될 때, 할당 받는 메모리 영역이다.
  - **Method Area** : **모든 쓰레드가 공유**하는 영역으로 class에 대한 정보가 저장된다. Class Loader가 클래스 파일을 읽어 오면 클래스 이름, 메소드와 필드 정보, 정적 변수 등을 보관한다.
   - **Heap** : **모든 쓰레드가 공유**하는 영역으로, 런타임에 생성된 모든 객체 인스턴스들이 저장되는 영역이다. 참조형(Reference Type) 데이터 타입을 갖는 객체(인스턴스), 배열이 저장되는 영역으로 GC가 참조되지 않는 메모리를 확인하고 제거한다.
   - **Stack** : 로컬변수, 매개변수, 중간 연산 결과들이 저장된다.
   - **PC Register** : 현재 실행되고 있는 스택 Frame의 주소를 저장하고 있는 영역이다. 멀티 쓰레드 프로그래밍 환경에서 한 쓰레드가 작업을 하다가 다른 쓰레드로 잠시 CPU 점유를 넘겨주고 돌아왔을 때 이전에 어떤 명령을 했는지에 대한 정보를 알 수 있다.
   - **Native Method Stack** : 자바 외 언어로 작성된 코드를 위한 메모리 영역
  
- **Execution Engine** : 바이트 코드(.class)를 기계어로 바꾸고 명령어 단위로 실행 및 바이트 코드를 운영체제에 맞게 해석해주는 역할을 수행한다. 크게 두 가지 방식이 사용된다.
  - 인터프리터 : 바이트 코드를 한 줄씩 읽고 실행한다. 컴파일보다 속도가 느리다.
  - JIT 컴파일: 인터프리터와 정적 컴파일 방식을 혼합한 것으로, 변환 작업은 인터프리터에 의해 수행되지만 필요한 코드의 정보는 캐시에 담아두었다가 재사용한다.

## JVM Stack과 Frame

![img5](https://github.com/dilmah0203/TIL/blob/main/Image/Stack%20Frame.png)

메소드를 실행하기 위해 필요한 정보가 저장되는 영역으로 **Stack은 Frame이라는 자료구조로 저장**이 되는데, **메소드가 호출될 때마다 생성되며 메소드가 끝나거나 예외 발생 시 소멸된다.**

Stack의 Frame은 다음과 같이 세 개의 부분으로 구성되어 있다.

1. **Local Variable Array** : 메소드 내의 로컬 변수들의 값을 배열로 담아놓고 인덱스로 접근이 가능하다. 배열의 첫 인덱스는 현재 인스턴스의 참조 this를 가지고 있다.
 
2. **Operand Stack** : 연산을 위한 작업 공간으로 연산의 중간값을 저장한다.

3. **Constant Pool Reference** : 클래스 내에 사용되는 상수들을 담는 영역으로, index와 type 그리고 매핑된 값으로 되어있다. JVM은 Constant Pool을 통해 해당 메소드나 필드의 실제 메모리 상 주소를 찾아 참조한다.

<br>

참고

https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html(https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html)



