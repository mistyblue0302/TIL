## B-Tree

B-Tree는 **탐색 성능을 높이기 위해 균형 있게 높이를 유지하는 Balanced Tree의 일종**이다.  모든 리프 노드가 같은 level로 유지되도록 자동으로 균형을 맞춰준다.
노드의 자식 수 중 최대값을 K라고 하면, 해당 B-Tree를 K차 B-Tree라고 한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree.png)

위 트리는 3차 B-Tree의 예시이다. 주황색은 자식 노드를 가리키는 포인터, 숫자 부분은 각 노드의 key이다. 

다음과 같은 특징을 같는다.

- 바로 위의 노드가 m개의 노드를 가질 때, 그 아래 자식 노드는 최소 m/2개 이상에서 최대 m개까지 자식 노드를 가질 수 있다.
- 노드의 key가 x개라면 자식의 수는 x+1개이다.
- key는 항상 정렬된 상태를 유지하며 이진 탐색 트리처럼 항상 각 key의 왼쪽 자식은 자신보다 작고, 오른쪽 자식은 자신보다 크다. 

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

- key가 들어갈 리프 노드를 검색 과정과 동일하게 탐색한다.
- 해당 리프 노드에 자리가 남아있다면 정렬을 유지하도록 알맞은 위치에 삽입하고, 리프 노드가 가득 차있다면 key를 삽입한 후 해당 노드를 분할한다.
- 노드 분할 시 노드의 중앙값을 기준으로 분할한다. 중앙값은 부모 노드로 합쳐지거나 새로운 노드로 생성된다.

삽입하고자 하는 값을 k라고 하였을 때, 리프노드가 가득차지 않았다면 오름차순으로 k를 삽입한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree8.png)

만약 리프노드가 가득찬 경우 노드를 분할한다.(중앙값을 기준으로) 중앙값은 부모 노드로 병합하거나 새로운 노드로 생성된다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree9.png)
![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree10.png)

## B-Tree의 key 삭제

특정 key를 삭제하는 경우도 트리의 균형 유지를 위해 변형이 필요하다. 삭제되는 key가 어느 노드에 속해있는지에 따라 삭제되는 방식이 조금씩 다르다.

### Case 1. 삭제할 key k가 리프에 있는 경우

**Case 1.1) 현재 노드의 key 개수가 최소 key 개수보다 크다면**

다른 노드들에 영향 없이 해당 k를 단순 삭제한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree16.png)
 
**Case 1.2) 왼쪽 또는 오른쪽 형제 노드의 key가 최소 key 개수 이상이라면**
  
1. 부모 key 값으로 k를 대체한다.
2. 최소키 개수 이상의 키를 가진 형제 노드가 왼쪽 형제라면 가장 큰 값을, 오른쪽 형제라면 가장 작은 값을 부모 key로 대체한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree17.png)

**Case 1.3) 왼쪽, 오른쪽 형제 노드의 key가 최소 key 개수이고, 부모노드의 key가 최소개수 이상이면**

1. k를 삭제한 후, 부모 key를 형제 노드와 병합한다.
2. 부모노드의 key개수를 하나 줄이고, 자식 수 역시 하나를 줄여 B-Tree를 유지한다

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree18.png)   

### Case 2. 삭제할 key k가 내부 노드에 있고, 노드나 자식의 key가 최소 key수보다 많을 경우

- `inorder predecessor` : 노드의 왼쪽 자손에서 가장 큰 key
- `inorder successor` : 노드의 오른쪽 자손에서 가장 작은 key
  
1. 현재 노드의 `inorder predecessor` 또는 `inorder successor`와 key의 자리를 바꾼다.
2. 리프 노드의 k를 삭제하게 되면, Case 1 조건으로 이동한다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree19.png) 

### Case 3. 삭제할 key k가 내부 노드에 있고, 노드에 key 개수가 최소 key 개수만큼, 노드의 자식 key 개수도 모두 최소 key 개수인 경우

1. k를 삭제하고, k의 양쪽 자식을 병합하여 하나의 노드로 만든다.
2. k의 부모 key를 인접한 형제 노드에 붙인다. 이후, 이전에 병합했던 노드를 자식노드로 설정
3. 해당 과정을 수행하였을 때 부모노드의 개수가 에 따라 이후 수행 과정이 달라진다. <br>
  3.1 만일 새로 구성된 인접 형제노드의 key가 최대 key 개수를 넘어갔다면, 삽입 연산의 노드 분할 과정을 수행 <br>
  3.2 만일 인접 형제노드가 새로 구성되더라도 원래 k의 부모 노드가 최소 key의 개수보다 작아진다면, 부모 노드에 대하여 2번 과정부터 다시 수행

![img](https://github.com/dilmah0203/TIL/blob/main/Image/B-Tree20.png) 

