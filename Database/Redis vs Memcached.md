### Disk based DB vs In-memory DB

- **Disk based DB**
  - HDD나 SDD 같은 디스크 저장 장치를 사용
    - 데이터를 디스크에 영구 저장
      - 시스템 재시작 시에도 데이터가 유지
      - 디스크 입출력 작업(I/O)이 메모리보다 느리기 때문에 데이터 접근 속도가 인메모리 DB에 비해 느리다.
      - MySQL, PostgreSQL
- **In-memory DB**
  - RAM을 사용
    - 데이터를 메모리에 저장
      - 휘발성 메모리라서 시스템이 재시작되면 데이터가 사라질 수 있다.
      - 메모리 속도가 디스크보다 빠르기 때문에 데이터 접근 및 처리 속도가 빠르다.
      - Redis, Memcached

## Redis vs Memcached

- **Memcached**
    - scale-up을 통해 확장
    - data eviction(제거) 전략이 서로 다르다. LRU(Least Recently User) 알고리즘만을 채택
    - 별도의 데이터 타입 없이 문자열을 저장
    - 데이터를 영속적으로 저장할 수 없어서 캐시로서의 기능만 수행 가능
- **Redis**
    - scale-out을 통해 확장
    - redis는 **싱글 쓰레드**로 동작하기 때문에 성능을 늘리는 것 보다는 redis를 여러 개 만들어 처리하는 것이 확장성에 더 좋고, memchaed는 **멀티 쓰레드**로 동작하기 때문에 성능을 올려 더 최적화를 시킨다는 의미인 것 같다.
    - 다양한 data eviction 전략이 있다.
    - 다양한 데이터 타입(String, Set, Sorted Set, Hash, List) 지원
    - 데이터를 영속적으로 저장이 가능하기 때문에 캐시외에 data store로서도 쓰일 수 있다.
        - 스냅샷: 순간적으로 메모리 있는 내용을 스냅샷을 찍어 디스크에 전체를 옮겨 담는다.
        - AOF: Redis에서 발생한 모든 write/update 등 연산들을 모두 log 파일에 기록

<br>

참고

[https://brownbears.tistory.com/43](https://brownbears.tistory.com/43)

[https://velog.io/@dnjwm8612/Session-Storage-Redis-Memcached](https://velog.io/@dnjwm8612/Session-Storage-Redis-Memcached)
