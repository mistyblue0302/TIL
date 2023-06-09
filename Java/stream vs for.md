## stream vs for

stream과 for는 다량의 데이터 처리 작업에 사용된다. 하지만 가독성이나 성능 등 여러 측면에서 차이가 있다.

### 표현 방식

- for문은 **코드 블록**으로 표현한다.
- stream은 **함수 객체**(람다식이나 메소드 참조)로 표현한다.

for문에서는 코드 블록의 외부 변수인 baseNumber에 대해 코드 블록 내부에서 수정이 가능하다.

```java
public int subtractByFor(int baseNumber, List<Integer> numbers) {
    for (int number : numbers) {
        if (number % 2 == 0) {
            baseNumber -= number;
        }
    }
    return baseNumber;
}
```

하지만 stream의 경우 람다식으로 표현을 하기 때문에 final인 변수만 읽을 수 있게 된다. 따라서 외부 변수인 baseNumber를 수정할 수가 없다.

```java
public int subtractByStream(int baseNumber, List<Integer> numbers) {
    numbers.stream()
        .filter(number -> number % 2 == 0)
        .forEach(nummber -> baseNumber -= number); //컴파일에러
    return baseNumber;
}
```

또한 for문은 return으로 메소드를 빠져나갈 수 있고, break나 continue를 통해 반복문을 제어할 수 있지만 람다로 표현하는 stream은 이와 같은 작업들이 불가능하다.

```java
public int myMethod(List<Integer> numbers) {
    int count = 0;
    int sum = 0;
    for (int number : numbers) {
        if (number % 2 == 0) {
            sum += number;
            if (number % 3 == 0) {
                continue;
            }
            count++;
        }
        if (count == 10) {
            break;
        }
    }
    return sum;
}
```

### 외부 반복(how) vs 내부 반복(what)

다음은 중복 체크에서 중복이 제거된 요소들의 개수를 구하는 코드이다.

for문을 이용하는 코드는 중복을 허용하지 않는 Set에 데이터들을 삽입하고 size() 메소드를 통해서 중복이 제거된 요소들의 개수를 구한다. 이는 구체적인 구현 로직이 외부에 노출되는 외부 반복 형식이다.

```java
public int byFor(List<Integer> numbers) {
    Set<Integer> duplicationRemoved = new HashSet<>();
    for (int number : numbers) {
        duplicationRemoved.add(number);
    }
    return duplicationRemoved.size();
}
```

stream을 이용하는 코드는 for문을 이용한 방식과는 달리 구체적인 구현 로직이 외부에 노출되지 않는 내부 반복의 형태를 띈다. 

```java
public int byStream(List<Integer> numbers) {
    return (int) numbers.stream()
        .distinct().count()
        .count();
}
```

외부 반복을 하는 for는 로직이 노출되어 있기 때문에 how 중심의 코드라고 할 수 있다. 반면 내부 반복형식의 stream은 로직이 추상화되어 있어 what 중심의 코드라고 볼 수 있다.

### 가독성

```java
public double average(List<Integer> grades) {
    int sum = 0;
    for(int grade : grades) {
        sum += grade;
    }
    return sum / grades.size();
}
```

```java
public double averageByStream(List<Integer> grades) {
    return grades.stream()
            .mapToInt(Integer::intValue)
            .average()
            .orElse(0.0);
}
```

for문의 경우 조건을 위해 if문의 중첩으로 인해 Indent Depth가 깊어져 가독성이 떨어질 수 있다. 하지만 stream의 경우 조건들을 filter의 체이닝 형식을 통해 표현이 가능하기 때문에 보다 간결한 표현이 가능하다.

### 디버깅

다음 코드들은 숫자들을 특정 값으로 나눠 합하는 로직을 for문과 stream으로 각각 구현한 것이다. 만약 매개변수 dividedNumber에 0이 들어온다면 DivideByZeroException이 발생한다. 

```java
private double divideByStream(List<Integer> divided, int dividedNumber) {
    return IntStream.range(0, divided.size())
        .mapToDouble(index -> divided.get(index) / dividedNumber)
        .sum();
}
```

stream을 통해 구현한 코드에서 예외가 발생하면 stream은 지연 연산으로 작업을 수행하기 때문에 실제 라인과는 다른 순서로 stack trace가 출력된다. 따라서 stream은 많은 내부 수행 작업과 지연 연산으로 인해 디버깅이 다소 어려울 수 있다.

```java
private double divideByFor(List<Integer> divided, int divideNumber) {
    double sum = 0.0;
    for (int a : divided) {
        sum += a / dividedNumber;
    }
    return sum;
}
```

반면 for문을 이용한 코드에서는 예외가 발생하는 stack trace가 간결하게 출력된다.

### 병렬처리

리스트의 사이즈가 커지거나 메소드가 많이 호출된다면 성능을 고려해야 할 것이다. 하지만 더 복잡한 로직의 경우 병렬로 처리하려면 직접 구현하기 까다롭다. 이럴 때 병렬 stream을 이용하면 내부적으로 처리해주기 때문에 보다 쉽게 병렬처리를 표현할 수 있다.

### 성능

- 배열 연산의 경우 for문이 stream보다 성능이 좋다.

for문의 경우 Java 1부터 등장했기 때문에 Java 8에 나온 stream에 비해 JVM에서 최적화가 많이 이루어졌다. 또한 stream을 사용하려면 stream 객체를 생성해야 하는데, stream에서 필요한 다른 객체를 생성하는데 오버헤드가 발생한다.

반면 for의 경우 추가적인 객체 생성 없이 Index를 통해서 메모리에 직접 접근하기 때문에 stream에 비해 오버헤드가 발생하지 않는다.

- 리스트의 경우 for문과 stream의 성능 차이는 크지 않다.

배열은 요소들이 원시타입인 반면에 컬렉션은 Wrapper 타입이 들어온다. 그래서 이와 같은 박싱(boxing)과 언박싱(unboxing)에 대한 오버헤드가 발생된다.

<br>

참고

[우아한Tech stream vs for](https://www.youtube.com/watch?v=by8hb75i9X4)
