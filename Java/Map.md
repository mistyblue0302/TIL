## Map

**Map**은 키(key)와 값(value) 쌍의 형태로 이루어져 있다. 

- 키를 값의 index로 사용한다.
- 순서가 없기 때문에 중간 삽입이 없다.
- 키는 중복이 불가능하지만 값은 중복이 가능하다. (중복이 있다면 마지막에 저장된 키가 출력)
- 키는 set에 저장된다.

```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);

for(String key : map.keySet()) {
    System.out.println(key + " " + map.get(key));
}

map.remove("a");
map.replace("b", 3);
map.put("c", 4); //hashmap에 키가 없는 경우 해당 키와 값을 삽입, replace는 null을 반환

int size = map.size();

for(String key : map.keySet()) {
    System.out.println(key + " " + map.get(key));
}
```

Map 인터페이스를 구현한 클래스는 다음과 같다

- HashMap
- TreeMap
- LinkedHashMap 

### HashMap

HashMap 클래스는 해시 알고리즘을 사용하여 검색 속도가 매우 빠르다. HashTable은 HashMap과 동일한 내부 구조를 가지고 있다. HashTable과의 차이점은 다음과 같다.

- thread-safe 여부
    - Hashtable은 Thread-safe하고, HashMap은 Thread-safe하지 않다. 멀티스레드 환경이 아니라면 Hashtable은 HashMap 보다 성능이 떨어진다.
- null 허용 
    - Hashtable은 key에 null을 허용하지 않지만, HashMap은 key에 null을 허용한다.

### TreeMap

### LinkedHashMap

LinkedHashMap은 데이터의 순서를 보장한다.


