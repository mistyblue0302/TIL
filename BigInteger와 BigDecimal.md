# 💡 **BigInteger와 BigDecimal**

> java.math 패키지
>
> 생성자를 사용하여 인스턴스를 생성
>
> 반드시 메소드를 사용

<br>

**BigInteger**

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
	}
}
```

<br>

**BigDecimal**

- 실수형 값을 다룰 때 사용

- 자리수 부족 또는 연산에서의 부정확성 문제를 해결

- 내부적으로 배열을 사용하여 데이터를 저장 -> 데이터 자리수에 제한이 없다

- 모든 연산은 메서드를 호출하여 수행

- 계산 속도는 double, float를 사용하는 경우보다 느리지만 정밀한 결과를 보장

```java
    BigDecimal e1 = new BigDecimal("100000.12345");
    BigDecimal e2 = new BigDecimal("10000");

    System.out.println("덧셈 :" + e1.add(e2));
    System.out.println("뺄셈 :" + e1.subtract(e2));
    System.out.println("곱셈 :" + e1.multiply(e2));
    System.out.println("나눗셈 :" + e1.divide(e2));
    System.out.println("나머지 :" + e1.remainder(e2));

    int compare = bigNumber1.compareTo(bigNumber2); //-1
	}
}
```
