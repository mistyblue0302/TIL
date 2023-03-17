## Set

- **Set**은 **입력 순서대로의 저장 순서를 보장하지 않는다.** 따라서 데이터의 존재 유무를 확인하기 위한 용도로 사용한다. 
- **데이터를 중복해서 저장할 수 없기 때문에** 같은 값을 저장하면 마지막에 저장한 값만 저장된다.

```java
Set<String> set = new HashSet<>();

set.add("hello world");
set.add("java");

boolean value = set.contains("java");
boolean value2 = set.isEmpty();

set.remove("java");
System.out.println(set.size());
}
```

List는 index로 관리하기 때문에 add()같은 메소드를 쓰면 순서대로 저장된다. Queue 또한 PriorityQueue를 제외하고는 기본적으로 입력한 순서대로 객체가 연결되어 있다. 하지만 Set의 경우는 입력받은 순서를 보장할 수 없다.

Set을 구현한 클래스는 3가지가 있다.

### HashSet

HashSet의 경우 Hash 알고리즘 기반으로 동작한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HashSet.png)

- 입력한 키를 hashcode() 메소드를 호출해 해시 코드로 변환한다. 해시 코드를 인덱스로 한 **bucket이라는 배열**에 해당 인덱스를 찾아서 저장한다.
- 이미 저장되어 있는 객체들의 해시 코드 값과 비교하여 동일한 해시 코드가 있다면, equals() 메소드로 두 객체를 비교 후 true면 저장하지 않는다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/HashSet2.png)

HashSet 객체를 만들면 내부적으로 HashMap 객체를 만들어서 사용한다. 

HashMap은 **기본 초기 용량 = 16, load factor = 0.75**인 배열로, 각 bucket은 해시 코드 값에 해당한다. **load factor**란, 데이터의 개수 / 저장공간으로 데이터의 개수가 증가하여 원래 load factor의 값보다 커지면 새 배열이 이전 배열의 두 배 크기로 생성되고 모든 요소가 새 bucket으로 재할당된다.

Hash에 의해 데이터의 위치를 특정시켜 해당 데이터를 빠르게 찾을 수 있다. 별도의 정렬 작업이 없으므로 순서가 없고 삽입, 삭제, 검색이 매우 빠르다. 예로 아이디 생성 시 중복확인을 눌러 중복된 아이디인지 확인하는 것이다. 이는 데이터가 정렬되어있을 필요도 없고 중복 값만 찾으면 되기 때문에 유용하다.

### LinkedHashSet

add() 메소드를 통해 요소들을 넣은 순서대로 연결한다. Set임에도 불구하고 **입력 순서대로의 저장순서를 보장**한다. 중복은 허용하지 않고 입력된 순서에 따라 **정렬**된다. 성능이 가장 좋지 않다. 만약 데이터의 중복을 허용하고 싶지 않지만 입력 순서를 보장받고 싶다면 LinkedHashSet을 사용하면 된다.

### TreeSet

이진트리의 향상된 버전인 Red-Black Tree를 기반으로 만들어진다. HashSet과 마찬가지로 중복 저장하지 않지만 저장된 데이터의 값에 따라 오름차순으로 **정렬**된다. 정렬 과정 때문에 HashSet보다 성능이 약간 느리다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/TreeSet.png)

- 새로운 데이터가 들어오면 루트부터 비교한다.
- 비교 결과에 따라 우측, 좌측으로 나뉜다.
- 더 이상 비교를 할 수 없을 때까지 반복한다.


