# 💡 **Map**

Map은 키(key)와 값(value)으로 이루어져 있다. Map 인터페이스를 구현한 클래스에는 HashMap, TreeMap, LinkedHashMap 등이 있다.

<br>

**HashMap의 생성자**

HaspMap에 객체가 들어가면 hashCode() 메소드 결과 값에 따른 bucket이라는 목록 형태의 바구니가 만들어진다. 서로 다른 키가 저장될 경우, hashCode()의 값이 동일하다면 bucket에 여러 값이 들어갈 수 있다. 따라서 get() 메소드 호출로 hashCode()의 결과를 확인 하고, bucket에 들어간 목록에 데이터가 여러 개일 경우 equals() 메소드를 통하여 동일 값을 찾게 된다.

<br>

Collection에서는 해당 위치에 값이 없을 경우 예외가 발생하지만 Map에서는 존재하지 않는 키로 get()을 할 경우 null을 리턴한다. 이미 존재하는 키로 값을 넣을 때에는 새로운 값으로 대치된다.

```java
Map<String, String> map = new HashMap<>();
map.put("A", "a");
map.put("C", "c");
System.out.println(map.get("A")); //a
System.out.println(map.get("B")); //null
```
<br>

**HashMap의 키를 확인하려면**

keySet() 메소드를 사용하여 키를 확인할 수 있다. 리턴 타입은 Set이다. 

```java
Map<String, String> map = new HashMap<>();
map.put("A", "a");
map.put("B", "b");
map.put("C", "c");
Set<String> keySet = map.keySet();
System.out.println(keySet); //[A, B, C]
for (String data : keySet) {
  System.out.print(map.get(data)); //abc
}
```        
<br>

**HashMap의 값을 확인하려면**

Set과 Map은 데이터 순서가 중요하지 않은 자료 구조이다. 객체에 담겨 있는 값만 필요할 경우 values() 메소드를 사용할 수 있으며 Collection 타입의 목록으로 리턴한다.

```java
Map<String, String> map = new HashMap<>();
map.put("A", "a");
map.put("B", "b");
map.put("C", "c");
Collection<String> values = map.values();
for (String data : values) {
System.out.print(data); //abc
}
```
Map에 선언된 Entry 타입 객체를 사용하여 리턴할 수도 있다. Entry는 하나의 키와 값만이 저장된다.

```java
Set<Map.Entry<String, String>> entry = map.entrySet();
for (Map.Entry<String, String> data : entry) {
System.out.println(data);
}
```


