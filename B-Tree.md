## B-Tree

B-Tree는 이진트리를 확장해 하나의 노드가 가질 수 있는 자식 노드 개수가 2보다 큰 트리구조이다. **탐색 성능을 높이기 위해 균형 있게 높이를 유지**한다. 모든 리프 노드가 같은 level로 유지되도록 자동으로 균형을 맞춰준다.
노드의 자식 수 중 최대값을 K라고 하면, 해당 B-Tree를 K차 B-Tree라고 한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree.png)

위 트리는 3차 B-Tree의 예시이다. 주황색은 자식 노드를 가리키는 포인터, 숫자 부분은 각 노드의 key이다. 
key는 항상 정렬된 상태를 유지하며 이진 탐색 트리처럼 항상 각 key의 왼쪽 자식은 자신보다 작고, 오른쪽 자식은 자신보다 크다. 
바로 위의 노드가 m개의 노드를 가질 때, 그 아래 자식 노드는 최소 m/2개 이상에서 최대 m개까지 자식 노드를 가질 수 있다. 

> Balanced tree를 사용하는 이유는 뭘까?

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree2.png)

일반적인 트리는 탐색에 평균 시간 복잡도 `O(logN)`을 갖지만 트리가 편향된 경우 최악에는 시간복잡도 `O(N)`을 갖게 된다. 루트 노드부터 탐색한다고 가정했을 때, 리프 노드까지 탐색한다면 모든 노드를 방문하기 때문에 `O(N)`이 될 수 있다.
이러한 단점을 보완하기 위해 항상 균형을 유지하는 트리가 필요하다. 최악의 경우에도 `O(logN)`의 시간이 보장된다.

## B-Tree의 key 검색

원하는 값을 K라고 하자.

- 루트 노드부터 탐색을 시작한다.
- 노드의 key를 순회하여 K가 존재하면 탐색을 종료한다.
- K가 존재하지 않는다면, 이웃한 두 key 사이에 K가 속하는 경우 포인터를 통해 자식 노드로 내려간다.
- 위 단계를 반복한다.

14라는 key를 검색한다고 해보자. 아래와 같이 진행된다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree3.png)

루트 노드의 키를 순서대로 확인한다. 

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree4.png)

14는 7보다 크고 15보다 작으므로 사이에 있는 포인터를 통해 자식 노드로 내려간다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree5.png)

자식으로 이동한 후 key를 순서대로 확인한다. 

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree6.png)

14는 11보다 크기 때문에 오른쪽에 있는 포인터를 통해 자식 노드로 이동한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree7.png)

마찬가지로 순서대로 탐색 후 14를 찾게된다.

## B-Tree의 key 삽입

key를 삽입하는 경우 트리의 균형이 깨질 수 있다.

- K가 들어갈 리프 노드를 검색 과정과 동일하게 탐색한다.
- 해당 리프 노드에 자리가 남아있다면 정렬을 유지하도록 알맞은 위치에 삽입하고, 리프 노드가 가득 차있다면 K를 삽입한 후 해당 노드를 분할한다.
- 노드 분할 시 노드의 중앙값을 기준으로 분할한다. 중앙값은 부모 노드로 합쳐지거나 새로운 노드로 생성된다.

13이라는 key를 삽입하는 과정이다. (검색 과정은 생략)

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree8.png)

12와 14사이에 삽입이 된다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree9.png)

노드에 들어갈 수 있는 key 수보다 더 많은 상태이다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree10.png)
![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree11.png)

리프 노드의 중앙값인 13이 부모 노드에 합쳐진다. 12와 14는 각각 13의 왼쪽, 오른쪽 자식이 된다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree12.png)

부모 노드 또한 key가 꽉 차있으므로 중앙값인 11을 분할한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree13.png)

루트 노드에 11이 합쳐졌다. 루트 노드 역시 key가 꽉 차있다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree14.png)

루트 노드의 중앙값인 11을 분할한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree15.png)

## B-Tree의 key 삭제

특정 key를 삭제하는 경우도 트리의 균형 유지를 위해 변형이 필요하다. 삭제되는 key가 어느 노드에 속해있는지에 따라 삭제되는 방식이 조금씩 다르다.

- 삭제할 key가 리프 노드에 있는 경우
  - 현재 노드의 key 개수가 최소 key 개보다 큰 경우
- 삭제할 key가 리프 노드를 제외한 노드에 있는 경우
  
 
  
  








