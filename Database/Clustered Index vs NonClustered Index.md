## Clustered Index

Clustered Index는 데이터가 테이블에 저장되는 순서를 설정한다. 즉, **특정 컬럼을 기준으로 데이터들을 정렬시킨다**. 테이블의 데이터는 오직 한 가지의 방법으로만 정렬되기 때문에 테이블 당 하나의 Clustered Index만 존재할 수 있다.

DB에서 Primary key 제약조건은 클러스터된 인덱스를 자동으로 생성하기 때문에 자료가 자동으로 정렬된다.

`이름` 컬럼을 기준으로 정렬하면 다음과 같이 Index Page가 생성된다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Clustered%20Index.PNG)

기존의 Data Page 데이터들은 이름을 기준으로 정렬이 되어 새로운 Data Page에 저장이된다. Index Page라는 별도의 Page에는 각각의 Data Page 번호와 첫 번째 데이터가 저장된다. 이 때 각각의 Data Page를 리프 페이지(Leaf Page)라고 하고, 각 리프 페이지의 번호와 첫 번째 데이터를 가지고 있는 Index Page를 루트 페이지(Root Page)라고한다. 

이름이 `이상준`인 데이터를 조회하면 다음과 같은 과정을 통해 데이터를 찾는다. 

1. Index Page에서 3행의 `장은비`보다 작기 때문에 그 이전인 `박유선`을 첫 번째 데이터로 가지는 Data Page 5를 찾아간다.
2. Data Page 5에서 차례대로 읽어가며 이름이 `이상준`인 데이터를 찾는다.

위와 같은 검색 방법을 인덱스 스캔(Index Scan)이라고 하며, Index Page를 참고해 특정 리프 페이지만을 검색하기 때문에, 전체 Data Page를 검색하는 테이블 스캔(Table Scan)보다 검색 속도가 훨씬 빨라진다.

- 어떤 경우에 생성해야 할까?
    - 테이블 업데이트가 적을 때
    - 읽기 작업이 많을 때 빠르다
    - 테이블은 정렬되어있기 때문에 ORDER BY 절을 활용해 모든 테이블 데이터를 스캔하지 않고 원하는 데이터를 조회할 수 있다.

## Non-Clustered Index

> Clustered Index에서 만약 이름이 아닌 휴대폰 번호로 검색할 경우 어떻게 될까? 

휴대폰 번호로는 정렬이 되어있지 않기 때문에  리프 페이지를 다시 처음부터 끝까지 하나하나 검색하는 Full Index Scan이 일어난다. Clustered Index는 정렬기준 컬럼으로 Data Page 자체를 새로 정렬해서 저장한다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Non-Clustered%20Index.PNG)

Non-Clustered Index는 Data Page를 전혀 건드리지 않고, '휴대폰 번호'를 기준으로 정렬된 Index Page를 별도로 생성해서 저장한다. Clustered Index와 마찬가지로 Root Index Page는 각각의 Leaf Index Page들의 시작 주소를 가지고 있다. Leaf Index Page의 Data Page 1:1는 해당 데이터가 몇 번째 Data Page의 몇 번 행에 있는지를 가리킨다. 즉, Index Page라는 것은 결국 주소만을 저장하고 있고, 데이터 자체는 Data Page에 저장한다.

휴대폰 번호가 `01064927878`인 데이터를 검색하면 다음과 같은 과정을 통해 데이터를 찾는다. 

1. Root Index Page에서 Leaf Index Page를 찾아간다.
2. Leaf Index Page에서 실제 데이터가 저장되어있는 Data Page를 찾아간다.

- 어떤 경우에 생성해야 할까?
    - 데이터가 자주 업데이트 될 때
    - where절이나 Join 절과 같은 조건문을 활용하여 테이블을 필터링 하고자할 때
