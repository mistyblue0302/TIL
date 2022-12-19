## BigInteger

Java의 int의 메모리 크기는 4Byte, long은 8Byte로 표현할 수 있는 범위가 정해져있다. 만약 해당 타입이 표현 가능 범위를 벗어나면 오버플로우(overflow)가 발생해 전혀 다른 값이 저장될 수 있다. 큰 정수를 다룰 경우, 고정된 타입보다는 자바에서 제공하는 java.math 패키지의 BigInteger 클래스를 활용할 수 있다.

- long 타입을 넘는 범위의 정수형 값을 다룰 때 사용
- 사칙연산을 할 수 없으며, 문자열을 이용하여 값을 표현
- 기호로 직접 연산할 수 없기 때문에 add(), subtract(), multiply(), divide(), compareTo() 와 같은 내장 메소드를 사용.

```java
    BigInteger e1 = new BigInteger("100000");
    BigInteger e2 = new BigInteger("10000");

    System.out.println("덧셈 :" + e1.add(e2));
    System.out.println("뺄셈 :" + e1.subtract(e2));
    System.out.println("곱셈 :" + e1.multiply(e2));
    System.out.println("나눗셈 :" + e1.divide(e2));
    System.out.println("나머지 :" + e1.remainder(e2));

    int compare = e1.compareTo(e2); //두 수 비교, 같으면 0 다르면 -1를 반환
```

## BigDecimal

부동소수점을 이용해 실수를 저장하는 float와 double은 소수점의 정밀도가 완벽하지 않다는 특징이 있다. 따라서 연산 시 정확한 값을 출력하지 않게 된다. 이유는 내부적으로 수를 저장할 때 근사치를 저장하기 때문이다. BigDecimal은 정밀한 결과를 보장한다.

- 실수형 값을 다룰 때 사용
- 실수 자료형이기 때문에 정밀도, 스케일, 부호 같은 속성을 추출하는 메소드가 존재
- 계산 속도는 double, float를 사용하는 경우보다 느리지만 정밀한 결과를 보장

```java
    BigDecimal e1 = new BigDecimal("123.45");
    BigDecimal e2 = new BigDecimal("-678.90");
     
    int a = e1.intValue(); // 형 변환
    long b = e1.longValue();
    float c = e1.floatValue();
    double d = e1.doubleValue();
    
    System.out.println(e1.precision()); //5, 정밀도. 0이아닌수가 시작하는 위치부터 0이아닌 수로 끝나는 자리수
    System.out.println(e1.scale()); //2, 소수점 첫째 자리부터 0이아닌 수로 끝나는 자리수
    System.out.println(e2.signum());  //부호 : -1(음수)
    
    int compare = e1.compareTo(e2); //두 수 비교 : -1
```

<br>

참고

https://www.baeldung.com/java-bigdecimal-biginteger
