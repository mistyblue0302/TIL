## HashTable

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HashTable2.png)

HashTable은 해시함수를 사용하여 key를 해시 값으로 매핑하고, 이 해시 값을 주소로 삼아 데이터(value)를 key와 함께 저장하는 자료구조이고 데이터가 저장되는 곳을 버킷이라고 한다. HashTable은 HashMap과 사용법이 거의 동일하다.

- **key**
  - 고유한 값으로 hash function의 input이 된다.
  - key값 그대로 저장소에 저장할 경우 다양한 key의 길이 만큼의 크기를 구성해두어야 하기 때문에 일정한 길이의 해시로 변경한다.
- **hash function**
  - key를 **고정된 길이**의 hash로 변경해주는 역할을 한다. 이 과정을 hashing이라고 한다.
  - 서로 다른 key가 같은 hash값을 가지게 되는 경우를 해시 충돌 이라고 한다.
- **value**
  - 저장소(bucket)에 최종적으로 저장되는 값으로, hash와 매칭되어 저장된다.

> key는 hash function을 통해 hash로 변경되며 hash는 value와 매칭되어 저장소에 저장 된다.

Hashtable 은 key-value가 1:1로 매핑되어 있기 때문에 삽입, 삭제, 검색의 과정에서 평균적으로 O(1)의 시간복잡도를 가지고 있다. 

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/HashTable.png)

위 처럼 기본 생성자로 객체를 생성하게 되면 `초기용량(버킷의 수) = 11`, `로드팩터 = 0.75`로 설정된다.

## HashMap과 HashTable 클래스의 차이점

- Thread-safe 여부
  - Hashtable은 Thread-safe하고, HashMap은 Thread-safe하지 않다는 특징을 가지고 있다. 그렇기에 멀티스레드 환경이 아니라면 Hashtable은 HashMap보다 성능이 떨어진다는 단점을 가지고 있다.
- Null 값 허용 여부
  - Hashtable은 key에 null을 허용하지 않지만, HashMap은 key에 null을 허용한다.
- HashMap은 보조 해시 함수를 사용하기 때문에 보조 해시 함수를 사용하지 않는 HashTable에 비해 해시 충돌이 덜 발생할 수 있어 상대적으로 성능상 이점이 있다. 
- [HashMap은 어떻게 구현되어 있을까?](https://dilmah0203.github.io/HashMap/)
