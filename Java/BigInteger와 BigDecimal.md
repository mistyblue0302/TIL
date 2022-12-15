## BigInteger와 BigDecimal

> java.math 패키지

### BigInteger

- long형을 넘는 범위의 정수형 값을 다룰 때 사용
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

### BigDecimal

- 실수형 값을 다룰 때 사용
- arithmetic(연산), scale manipulation(범위 조정), rounding(반올림), comparison(비교), hashing(해싱), and format conversion(포멧 변경)을 지원
- 계산 속도는 double, float를 사용하는 경우보다 느리지만 정밀한 결과를 보장


```java
    BigDecimal e1 = new BigDecimal("123.45");
     
    int a = e1.intValue(); // 형 변환
    long b = e1.longValue();
    float c = e1.floatValue();
    double d = e1.doubleValue();
    
    System.out.println(e1.scale()); //2, 소수점 첫째 자리부터 0이아닌 수로 끝나는 자리수
    System.out.println(e1.precision()); //5, 정밀도. 0이아닌수가 시작하는 위치부터 0이아닌 수로 끝나는 자리수
    
    int compare = bigNumber1.compareTo(bigNumber2); //두 수 비교 : -1
```
