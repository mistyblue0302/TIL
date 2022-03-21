# 💡 **java.lang 패키지**

Java 패키지 중 유일하게 import 없이 사용가능하다.

<br>

**숫자를 처리하는 클래스**

기본 자료형은 Stack 영역에 저장되어 관리된다. Stack은 이미 할당되어 있는 공간을 사용하므로 보다 빠른 처리가 가능하다. 그런데 기본 자료형을 객체로 변환해야 되는 경우를 위해 Java에서는 기본 자료형으로 선언되어 있는 타입의 클래스들이 있다. Character와 Boolean을 제외한 나머지 클래스들을 Wrapper 클래스라고 한다.

- Byte
- Short
- Integer
- Long
- Float
- Double
- Character
- Boolean

 Character 클래스를 제외하고는 parse타입()메소드와 valueOf()메소드를 공통적으로 가지고 있다.


 ```java
public void Example() {
        String value1 = "2";
        String value2 = "3";
        byte byte1 = Byte.parseByte(value1);
        byte byte2 = Byte.parseByte(value2);
        System.out.println(byte1 + byte2); //5

        Integer int1 = Integer.valueOf(value1);
        Integer int2 = Integer.valueOf(value2);
        System.out.println(int1 + int2 + "6"); //56
    }
 ```

 String 값인 2와 3을 parseByte()메소드를 사용하여 byte로 변환해주었다. 그리고 valueOf()메소드를 사용하여 Integer타입으로 변환하였다.

 두 메소드는 static 메소드이므로 객체 생성 없이도 실행이 가능하다.
 parse타입() 메소드는 기본 자료형을 반환하지만, valueOf()메소드는 참조 자료형을 반환한다.

 <br>

 **System 클래스**

 모든 멤버가 static이기 때문에 별도의 객체를 생성하지 않아도 된다. 표준 입출력과 현재시간 조회등에 사용된다. System 클래스에서 제공되는 메소드는 다양하다.

 - 시스템 속성(Properties)값 관리
 - 시스템 환경(Environment) 조회
 - 현재 시간 조회
 - GC 수행
 - JVM 종료

 Properties는 java.util 패키지에 속하며, Hashtable의 상속을 받는다 Properties는 추가와 변경이 가능하지만 환경값 env는 읽기만 가능하다
