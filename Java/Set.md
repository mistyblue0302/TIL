## Set

- **Set**은 **순서와 상관없이**, 데이터의 존재 유무를 확인하기 위한 용도로 사용한다. 
- **중복을 허용하지 않기 때문에** 같은 값을 저장하면 마지막에 저장한 값만 저장된다.

```java
Set<String> set = new HashSet<>();

set.add("hello world");
set.add("java");

Iterator<String> iterator = set.iterator();

while (iterator.hasNext()) {
    String s = iterator.next();
    System.out.println(s);

}
```

### HashSet

HashSet의 경우 Hash 알고리즘 기반으로 동작한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HashSet.png)

- 입력한 키를 hashcode() 메소드를 호출해 해시 코드로 변환한다. 이미 저장되어 있는 객체들의 해시 코드 값과 비교하여 동일한 해시 코드가 있다면, equals() 메소드로 두 객체를 비교 후 true면 저장하지 않는다.
- 해시 코드를 인덱스로 한 bucket이라는 배열에 해당 인덱스를 찾아서 저장한다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/HashSet2.png)

HashSet 객체를 만들면 내부적으로 HashMap 객체를 만들어서 사용한다. 기본 초기 용량 = 16, load factor = 0.75이다. load factor란, 데이터의 개수 / 저장공간으로 데이터의 개수가 증가하여 원래 load factor의 값보다 커지게 되면 해시 재정리 작업이 수행된다.

HashSet 클래스의 생성자를 통해 초기 용량, load factor를 지정할 수 있다.


별도의 정렬 작업이 없으므로 가장 성능이 좋다.


### TreeSet

저장된 데이터의 값에 따라 **정렬**된다. red-black 트리 타입으로 값이 저장되며 HashSet보다 성능이 약간 느리다.

### LinkedHashSet

저장된 **순서**에 따라 정렬된다. 성능이 가장 좋지 않다.

성능 차이가 발생하는 이유는 데이터 정렬 때문이며, HashSet은 별도의 정렬 작업이 없기 때문에 제일 빠르다.
