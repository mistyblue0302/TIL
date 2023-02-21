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
- 해시 코드를 인덱스로 한 **bucket이라는 배열**에 해당 인덱스를 찾아서 저장한다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/HashSet2.png)

HashSet 객체를 만들면 내부적으로 HashMap 객체를 만들어서 사용한다. 

HashMap은 **기본 초기 용량 = 16, load factor = 0.75**인 배열로, 각 bucket은 해시 코드 값에 해당한다. **load factor**란, 데이터의 개수 / 저장공간으로 데이터의 개수가 증가하여 원래 load factor의 값보다 커지면 새 배열이 이전 배열의 두 배 크기로 생성되고 모든 요소가 새 bucket으로 재할당된다.

별도의 정렬 작업이 없으므로 순서가 없고 가장 성능이 좋다.

### TreeSet

이진트리의 향상된 버전인 Red-Black Tree를 기반으로 만들어진다. 

HashSet과 마찬가지로 중복 저장하지 않지만 저장된 데이터의 값에 따라 오름차순으로 **정렬**된다. 정렬 과정 때문에 HashSet보다 성능이 약간 느리다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/TreeSet.png)

- 새로운 데이터가 들어오면 루트부터 비교한다.
- 비교 결과에 따라 우측, 좌측으로 나뉜다.
- 더 이상 비교를 할 수 없을 때까지 반복한다.

### LinkedHashSet

중복은 허용하지 않지만 **입력된 순서**에 따라 정렬된다. 성능이 가장 좋지 않다.

