세션 불일치 문제를 해결하기 위해 별도의 session storage를 구성하여 세션의 정합성 문제를 해결하려고 할 때, 과연 세션 스토리지로 적합한 DB는 무엇이 있을까?
우선 DB는 크게 RDB와 No-SQL을 생각할 수 있다.

- RDB
  - Oracle Database
  - MySql
  - PostgreSQL
- No-SQL
  - MongoDB
  - Redis
 
만약 인증된 사용자만 접근할 수 있는 요청을 처리할 때는 매번 세션 저장소에서 해당 로그인 세션이 존재하는지 확인하는 작업이 필요하므로 빠르게 세션 정보를 찾아서 제공해야 한다.

## Disk based DB vs In-memory DB

그렇다면 세션 정보를 빠르게 제공해주기 위해서는 데이터를 어디에 저장해야 할까? 저장 위치에 따라 Disk based DB vs In-memory DB로 나뉜다. Disk based DB는 데이터를 
디스크에 저장하여 관리하고 In-memory DB는 데이터를 메모리에 저장하여 관리한다.

### Disk based DB

먼저 Disk based DB의 접근 과정을 알아보자. 우선 메모리에 버퍼라는 임시 저장공간에서 저장된 데이터를 먼저 탐색 후 해당 데이터가 없을 때만 디스크에 저장된 데이터를 버퍼에 읽어오는 방식이다.
이러한 방식을 사용하는 이유는 메모리의 I/O 작업이 디스크 I/O 작업보다 약 10,000배 이상 빠르기 때문이다.

하지만 디스크에서 데이터를 찾아 버퍼로 읽어오는 시간이 발생하기 때문에 여러 I/O 작업에서 병목 현상이 발생하게 된다. 그렇기 때문에 세션의 존재를 반복적으로 확인하는 작업이 필수적인 서비스에서는
세션 스토리지로 Disk based DB를 사용하는 것은 성능적인 측면에서 어렵다.

### In-memory DB

In-memory DB는 디스크의 I/O 작업이 필요없이 모든 데이터를 메모리에 저장하기 때문에 디스크 기반의 DB에서 발생하는 병목 현상을 피할 수 있다. 그렇다면 병목이 발생하지 않고 빠르기 때문에 항상
Disk based DB보다 In-memory가 좋을까?

그것은 사용하는 목적과 상황에 따라 다르다.

우선 In-memory DB는 기본적으로 영속성을 보장하지 않는다. 예기치 못하게 프로세스가 종료된 경우, 현재 데이터가 모두 유실될 수 있다. 그렇다면 세션 저장소로는 적합할까?

세션은 HTTP의 비연결지향과 무상태성과 같은 특성을 보완하기 위해 사용한다. 세션은 주로 로그인한 사용자의 정보를 저장하는데, 이 정보는 사용자가 로그아웃을 하거나 개발자가 설정한
세션의 timeout에 따라서 제거된다. 세션에 저장되는 데이터는 이러한 특성 때문에 데이터 유실로 인한 피해가 다른 데이터에 비해 적다. 그리고 In-memory DB는 또 다른 세션 스토리지에 복사하는 마스터 - 슬레이브 복제 방법을 사용해
가용성을 보장하기도 한다. 따라서 세션 저장소로 In-memory DB의 사용이 적절하다고 생각한다.

In-memory DB에는 다양한 DB들이 존재하는데, 세션 저장소로 가장 많이 사용되는 `Redis`와 `Memcached`에 대해 알아보자.

## Redis

- 다양한 데이터 타입(String, Set, Sorted Set, Hash, List) 지원
- 데이터 영속성을 보장하기 위해 데이터를 디스크에 저장하여 메모리에 로딩하는 방법을 제공
  - snapshotting : 순간적으로 메모리 있는 내용을 스냅샷을 찍어 디스크에 전체를 옮겨 담는 방식
  - AOF(Append On File) : Redis에서 발생한 모든 write/update 등 연산들을 모두 log 파일에 기록
- 다양한 데이터 구조(String, Set, Sorted Set, Hash, List) 지원
- 장애가 발생한 경우 Replication을 통한 간편한 서버 복구 기능
- 싱글쓰레드로 동작
  
## Memcached

- 데이터가 메모리에만 저장된다. 데이터는 프로세스가 종료되면 사라진다.
- 데이터를 문자열로만 저장
- 멀티쓰레드로 동작

### 정리

Memcached는 데이터를 문자열로만 저장하는 반면, Redis는 다양한 자료형을 지원한다. 따라서 Memcached를 사용할 때는 데이터를 전부 변환하여 저장하고, 가져올 때 파싱이 필요하지만 Redis를 사용할 때는 바로 원하는 데이터로 다룰 수 있다. 그리고 Memcached는 프로세스가 종료되면 임시 저장되었던 데이터가 사라지지만 Redis는 snapshot과 AOF 기능을 통해 데이터 유실 없이 복구할 수 있다. Redis는 Replication을 통해 다른 서버에도 동일한 데이터가 보관될 수 있게 하여 서버 복구가 가능하다.

<br>

참고

[https://brownbears.tistory.com/43](https://brownbears.tistory.com/43)

[https://velog.io/@dnjwm8612/Session-Storage-Redis-Memcached](https://velog.io/@dnjwm8612/Session-Storage-Redis-Memcached)
