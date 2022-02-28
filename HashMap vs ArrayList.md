# 💡 **HashMap vs ArrayList**

- **HashMap**

  key와 value를 묶어 하나의 entry로 저장한다는 특징을 가지고 있다. key는 중복을 허용하지 않고, value는 중복을 허용한다. 동일한 key가 입력되면 나중에 입력된 데이터(key, value)로 덮어씌워진다. 해싱(Hasing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다.
  key와 value에 null을 허용한다.

  매핑으로 인해 삽입, 삭제, 검색이 평균적으로 O(1)인 자료구조이다. 최악의 경우는 O(n)이 될 수 있다.

  HashMap은 저장공간을 초과하여 값이 추가로 들어오면 List처럼 저장공간을 추가로 늘리는데, List처럼 한 칸씩 늘리지 않고 약 두배로 늘리게된다.

<br>

```java
HashMap<Integer,String> map = new HashMap<Integer,String>();//초기값 지정
map.put(1,"a");
map.put(2,"b");
map.put(3,"c");
map.get(1); //데이터 추출
String val = map.get(2).toString(); //key로 값 가져오기
map.remove(1); //key값 1 제거
map.clear(); //모든 값 제거
System.out.println(map.containsKey(1)); //해당 키:1 가 있는지 확인
System.out.println(map.containsValue("a")); // 해당 value :a 가 있는지 확인
```

빅오표기 <br>

put : O(1) <br>
get : O(1)

<br>

- **ArrayList**

  List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 선형리스트이다. 내부구현은 배열로 되어있으며 순차적으로 데이터를 저장하고 인덱스로 내부의 객체를 관리한다. 배열과는 달리 ArrayList는 객체들이 추가되는만큼 저장 용량이 늘어난다는 특징이 있다.

  데이터 추가와 삭제에 유용하지 못하지만, 순차적인 추가와 삭제는 제일 빠르다. 또한 인덱스가 있어 임의의 요소에 대한 접근성이 뛰어나다.
  하지만 대량의 자료를 추가/삭제 하는 경우에 상당한 성능저하를 가져오며, 많은 데이터를 한번에 다 가져와 참조해 쓸 때 최상의 성능을 나타낸다.

<br>

```java
ArrayList<String> list = new ArrayList<String>();
list.add("apple"); //값 추가
list.add(null); //null값도 add가능
list.add(2,"orange"); //index 2에 orange삽입
list.remove(1);  //index 1 제거
list.clear();  //모든 값 제거
list.set(0, "오렌지"); //데이터 수정
int index = list.indexOf("apple"); //인덱스 알아내기
```

빅오표기 <br>

add : O(1) <br>
remove : O(n) <br>
get : O(1) <br>
contains : O(n)

<br>

- **HashMap은 어떻게 구현되어있길래 키와 값을 빠르게 매핑할 수 있을까?**
  <br>

  HashMap은 key를 이용하여 값을 찾을 수 있는데, 내부구조는 배열로 되어있고 key는 배열의 인덱스가 될 수 있다. 이 배열을 버킷이라 한다.

  <br>

> capacity = bucket의 수
>
> 한 HashMap 객체에 저장된 데이터 수 = load_factor

<br>

인덱스를 구하기 위해서는 hashcode() 메소드를 사용하여 값이 저장될 버킷을 결정하며, 내부적으로 key값의 equals()를 사용하여 정확히 일치하는 항목을 찾는다. <br>

HashMap은 어떻게 데이터 접근에 O(1)을 보장할 수 있을까?
index를 확실히 알고 있는 배열에 접근하기 때문이다.

여기서 bucket의 수는 배열의 크기이며, HashMap을 기본적으로 선언하게 되면 default bucket의 크기는 16으로 지정이 된다. 즉 16개의 Node<K, V>가 들어갈 수 있는 배열이 선언된 것.

위에서 key를 알맞은 버킷에 저장하기 위해 hashCode를 사용한다고 했는데, 바로 hashCode를 버킷 수로 나눈 나머지를 배열의 index로 사용한다.

따라서 HashMap을 생성하게 되면, 나머지는 0~15중 하나가 되며 해당 index에 key-value를 저장할 수 있다.
