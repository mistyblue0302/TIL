# 💡 **List**

**배열**

동일한 자료형의 데이터를 연속된 공간에 저장하는 구조로, 다음과 같은 단점이 있다.

- 자료형이 같아야한다
- 크기가 고정되어 있다

이러한 단점을 보완하기 위해 List가 생겼다.

<br>

**List**

데이터가 **순서가 있으며 중복을 허용한다**

**ArrayList**

- 내부적으로 배열을 이용하여 데이터를 저장한다
- Thread safe하지않다

<br>

ArrayList 객체 선언 시 초기 크기는 **10**이다. 예를들어 String인 객체를 담는 크기가 30인 ArrayList를 생성할 때에는 다음과 같이 사용한다.

```java
ArrayList<String> list = new ArrayList<String>(30);
```

<br>

list2를 list로 치환할 경우, list2가 list의 값과 주소까지 사용하게 된다.

```java
 ArrayList<String> list = new ArrayList<String>();
        list.add("A");

        ArrayList<String> list2 = list;
        list.add("B");
        for (String data : list2) {
            System.out.print(data); //A B
        }
```

<br>

ArrayList 객체에 있는 데이터를 배열로 뽑아낼 경우 toArray()메소드를 사용한다. 매개 변수가 없는 toArray() 메소드는 Object 타입 배열로만 리턴을 하기 때문에 제네릭을 사용하여 선언한 ArrayList 객체는 매개변수로 타입을 정해준다.

```java
  ArrayList<String> list = new ArrayList<>();
        list.add("C");
        list.add("D");
        String[] array = new String[0];
        String[] array2 = list.toArray(array);

        for (String data : array2) {
            System.out.print(data);
        }
```
ArrayList 객체의 크기가 매개변수로 넘어간 배열 객체의 크기보다 클 경우 배열의 모든 값이 null이 된다. 따라서 toArray() 메소드에 크기가 0인 배열을 넘겨주는 것이 좋다.

<br>

**LinkedList**

- ArrayList의 경우 특정 인덱스의 객체를 제거할 시 나머지 모든 객체가 앞으로 1칸씩 이동한다. 객체 추가시에는 모든 객체가 뒤로 1칸씩 밀려난다. 따라서 객체의 삭제와 삽입이 빈번하게 일어 날 경우 LinkedList를 사용하는 것이 좋다.
- ArrayList는 인덱스로 데이터를 관리하지만, LinkedList의 경우 인접한 곳을 링크하여 관리한다. 중간에 데이터 삭제 시 링크만 변경하면 되기 때문에 처리속도가 빠르다.


**Vector**

- ArrayList 와의 차이점은 동기화 유무다. 때문에 ArrayList 보다 객체를 추가, 삭제하는 과정이 느리다.

