# 💡 **Map vs List**

> Java Collection Framework
>
> Java에서 기본적인 자료구조를 제공하기 위한 환경
>
> java.util 패키지에 속해 있다

<br>

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcHhmbr%2FbtqwUDHGMOV%2FusCxTFLWrG0iSkJzqdK8WK%2Fimg.png)

<br>

- **Map**

  - 키(key), 값(value)의 쌍으로 이루어진 데이터의 집합으로, 순서는 유지되지 않으며 키의 중복을 허용하지 않으나 값의 중복은 허용한다.

  - **Hashtable**

    HashMap보다는 느리고, null 불가

  - **HashMap**

    내부적으로 배열을 만들어 관리 <br>
    key값으로 넘겨준 객체의 해시 코드를 계산하여 배열의 접근 인덱스로 사용 <br>
    해시 충돌의 경우를 대비해 equals()메소드 사용 <br>
    중복과 순서가 허용되지 않고, null 값이 올 수 있다 <br>

  - **TreeMap**

    Red-Black 트리 기반 NavigableMap 구현 <br>
    정렬된 순서대로 키와 값을 저장하여 검색이 빠름 <br>
    데이터 탐색 시 O(log N)의 시간이 걸린다. <br>

<br>

- **List**

  - 순서가 있는 데이터의 집합으로 데이터 중복을 허용한다.

  - **LinkedList**

    양방향 포인터 구조로 데이터 삽입, 삭제가 빈번할 경우 유용

  - **ArrayList**

    단방향 포인터 구조로 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남

  - **Vector**

    과거에 대용량 처리를 위해 사용

