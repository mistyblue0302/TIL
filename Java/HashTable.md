## HashTable

HashTable 클래스는 Collections Framework가 만들어지기 전부터 존재했다. Vector나 HashTable과 같은 기존의 클래스들은 호환을 위해 남겨두었지만 가능한 ArrayList나 HashMap을 사용하는 것이 좋다.

HashTable은 HashMap과 사용법이 거의 동일하다.(key-value의 형태이고 key는 중복될 수 없고, value는 중복될 수 있다.)

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HashTable.png)

위 처럼 기본 생성자로 객체를 생성하게 되면 `초기용량(버킷의 수) = 11`, `로드팩터 = 0.75`로 설정된다.

## HashMap과 HashTable 클래스의 차이점

- Thread-safe 여부
  - Hashtable은 Thread-safe하고, HashMap은 Thread-safe하지 않다는 특징을 가지고 있다. 그렇기에 멀티스레드 환경이 아니라면 Hashtable은 HashMap보다 성능이 떨어진다는 단점을 가지고 있다.
- Null 값 허용 여부
  - Hashtable은 key에 null을 허용하지 않지만, HashMap은 key에 null을 허용한다.
- HashMap은 보조 해시 함수를 사용하기 때문에 보조 해시 함수를 사용하지 않는 HashTable에 비해 해시 충돌이 덜 발생할 수 있어 상대적으로 성능상 이점이 있다. 
- [HashMap은 어떻게 구현되어 있을까?](https://dilmah0203.github.io/HashMap/)
