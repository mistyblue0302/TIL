# 💡 **HashMap vs ArrayList**

- **HashMap**

  - key와 value를 묶어 하나의 entry로 저장한다는 특징을 가지고 있다.
  - 하나의 key는 하나의 value
  - 해싱(Hasing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다.
  - 매핑으로 인해 삽입, 삭제, 검색이 평균적으로 O(1)인 자료구조이다. 최악의 경우는 O(n) 이 될 수 있다.

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

  - List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 선형리스트이다.
  - 내부구현은 배열로 되어있으며 순차적으로 데이터를 저장하고 인덱스로 내부의 객체를 관리한다.
  - 배열과는 달리 ArrayList는 객체들이 추가되는만큼 저장 용량이 늘어난다는 특징이 있다.
  - 데이터 추가와 삭제에 유용하지 못하지만, 순차적인 추가와 삭제는 제일 빠르다.
    하지만 대량의 자료를 추가/삭제 하는 경우에 상당한 성능저하를 가져온다.

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

- **HashMap의 내부구현**
  <br>
  
  HashMap은 key를 이용하여 값을 찾을 수 있는데, 내부구조는 배열로 되어있고 key는 배열의 인덱스가 될 수 있다. 이 배열을 버킷이라 한다.

  <br>

> capacity = bucket의 수
>
> 한 HashMap 객체에 저장된 데이터 수 = load_factor

<br>

여기서 bucket의 수는 배열의 크기이며, HashMap을 기본적으로 선언하게 되면 default bucket의 크기는 16으로 지정이 된다.

인덱스를 구하기 위해서는 hashcode() 메소드를 사용하여 값이 저장될 버킷을 결정하며, 내부적으로 key값의 equals()를 사용하여 정확히 일치하는 항목을 찾는다. <br>

**HashMap은 어떻게 데이터 접근에 O(1)을 보장할 수 있을까?**

index를 확실히 알고 있는 배열에 접근하기 때문이다.
위에서 key를 알맞은 버킷에 저장하기 위해 hashCode를 사용한다고 했는데, 바로 hashCode를 버킷 수로 나눈 나머지를 배열의 index로 사용한다.

따라서 HashMap을 생성하게 되면, 나머지는 0~15중 하나가 되며 해당 index에 key-value를 저장할 수 있다.

ArrayList와 HashMap은 equals 비교를 통해 원하는 값을 찾아와야 하는 점은 같다. <br>
ArrayList는 데이터를 찾기 위해 처음부터 끝까지 전체 루프를 돌아야 하지만, HashMap은 key의 해시값을 통해 버킷의 번호를 찾고, 해당 버킷 안에서 몇번의 equals비교로 데이터를 찾을 수 있다.

<br>

- **해시충돌**

  **서로 다른 키를 가진 레코드들이 하나의 버킷에 매핑되는 경우**를 해시충돌 이라고 한다.

  해시충돌이 발생하게 되면 검출 속도를 느리게 만들고 버킷 오버플로우를 발생시킨다.

  버킷 오버플로우란? 만약 버킷의 크기가 1인 경우 서로 다른 2개 이상의 key를 해시함수하여 같은 해시값이 나오면 버킷의 크기를 넘어서 저장하게 된다.
  <br>

  **문제점** <br>
  버킷의 크기를 넘어 저장하는 경우 다른 버킷이 사용하는 주소 공간을 침범하여 사용할 수 있다.

<br>

- [**해시충돌을 해결하기 위한 방법**](https://d2.naver.com/helloworld/831311)
