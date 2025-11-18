# CS 면접을 위한 자료구조와 알고리즘 질문리스트

## Algorithm

## Array와 Linked List
<details>
  <summary>자료구조는 무엇이고 왜 쓰는건가요?</summary>
  <ul>
    <li> 자료구조는 데이터를 효율적으로 저장하고 관리하기 위한 구조적 방식입니다. 단순히 데이터를 모아두는게 아니라, 어떤 방식으로 저장하면 더 빠르게 찾고, 삽입하고, 삭제할 수 있는지를 고민한 결과가 자료구조입니다. 상황에 맞는 자료구조를 선택하는것이 성능과 안정성에 큰 영향을 미칩니다.</li>
  </ul>
</details>

<details>
  <summary>Array는 어떤 자료구조 인가요?</summary>
  <ul>
    <li> Array는 연관된 데이테를 메모리상에서 연속적이며 순차적으로 미리 할당된 크기만큼 저장하는 자료구조입니다. 논리적 저장 순서와 물리적 저장 순서가 일치합니다. 특징으로는 고정된 저장공간, 순차적인 데이터 저장이 있습니다.</li>
    <li> Array는 조회와 append와 장점이 있어 조회가 자주 일어나는 상황에 유리합니다. 하지만 고정된 크기를 가지고 있어 메모리 낭비나 추가적인 오버헤드가 발생할 수 있다는 단점이 있습니다.</li>
  </ul>
</details>

<details>
  <summary>Array의 시간 복잡도에 대해 아시나요?</summary>
  <ul>
    <li> Array는 접근, 맨 끝 원소에 추가하거나 삭제하는 작업은 O(1)의 복잡도를 가지며, 중간에 삽입하거나 삭제하고, 검색하는 경우에는 O(N)의 시간복잡도를 가집니다.</li>
  </ul>
</details>

<details>
  <summary>미리 예상한것 보다 더 많은 수의 데이터를 저장하느라 Array의 size를 넘어서게 되었습니다. 이 때, 어떻게 해결할 수 있나요?</summary>
  <ul>
    <li> 기존의 크기보다 더 큰 배열을 선언하여 데이터를 옮겨 할당합니다. 모든 데이터를 옮겼다면 기존 배열은 메모리에서 삭제합니다. 이런식으로 동적으로 배열의 크기를 조절하는 자료구조를 Dynamic Array라고 합니다. 또 다른 방법으로는 사이즈를 예측하기 쉽지 않다면 배열 대신 Linked List를 사용함으로서 데이터가 추가될 때마다 메모리 공간을 할당받는 방식을 사용합니다.</li>
  </ul>
</details>

<details>
  <summary>Dynamic Array는 어떤 자료구조인가요?</summary>
  <ul>
    <li> Array의 경우 size가 고정되어있기 때문에 선언시에 설정한 사이즈보다 더 많은 데이터를 추가할 수 없습니다. 이에 반에 다이나믹 어레이는 Array의 사이즈가 가득차게 되면 자동으로 리사이즈를 수행하여 유동적으로 사이즈를 조정하여 데이터를 저장하는 자료구조입니다.</li>
    <li> resize를 하는 방식에는 대표적으로 doubling이 있습니다. 기존의 배열보다 2배 큰 배열을 선언하여 데이터를 일일히 옮기는 방법이며 O(n)의 시간복잡도를 갖습니다. </li>
    <li> Dynamix Array에 원소를 추가하는것은 O(1)이지만 리사이즈가 일어나면 O(n)이 간간히 일어납니다. 따라서 append의 시간복잡도는 Amortized O(1)이라고 합니다.</li>
  </ul>
</details>

<details>
  <summary>Dynamic Array와 Linked List와 비교해서 장단점을 설명해주세요. </summary>
  <ul>
    <li> 링크드리스트와 비교했을때 다이나믹 배열의 장점은 데이터 접근과 할당이 O(1)로 빠릅니다. 이는 index에 접근하는 방식이 첫 data의 주소값 + offset으로 되어 있어 랜덤 엑세스가 가능하기 때문입니다. 또한 맨 뒤에 데이터를 추가하는것이 빠릅니다.</li>
    <li> 단점은 다이나믹 메모리의 맨끝이 아닌 곳에 데이터를 삽입하거나 삭제하는 작업이 O(N)으로 느린편입니다. 메모리가 연속적으로 저장되어 있기 때문에 shift 연산이 추가적으로 필요하기 때문입니다. 또한 resize시 성능이 떨어지고, 시간이 많이 걸려 필요한것 이상의 memory를 할당받게 되어 낭비되는 공간이 생깁니다. </li>
  </ul>
</details>

<details>
  <summary>Linked List에 대해 설명해주세요.</summary>
  <ul>
    <li> 링크드리스트는 노드라는 구조체를 가지고있는데, 각 노드는 데이터의 값과 다음 노드의 주소값을 가지고 있습니다. 링크드리스트는 물리적으로는 비연속적인 위치에 저장되지만 각 노드가 다음 데이터의 주소를 가지고 있음으로서 논리적으로 연속적으로 저장됩니다.</li>
    <li> 장점으로는 주소를 가지고있기 때문에 그것만 다른값으로 바꿔주면 삭제와 삽입을 O(1) 시간에 해결할 수 있으며 메모리 사용이 자유롭다는 점이 있습니다. 단점으로는 원하는 위치에 삽입하려면 search하는 과정을 통해 스캔해야 해서 O(N)의 시간이 추가적으로 소요되며 다음 주소를 저장해야하기때문에 데이터 하나당 차지하는 메모리가 더 커진다는 점이 있습니다.</li>
  </ul>
</details>

<details>
  <summary>Linked List의 시간복잡도 에 대해 설명해주세요.</summary>
  <ul>
    <li> 링크드 리스트는 access와 검색에 o(n)의 시간이 걸리며, insertion과 delete에는 O(1)의 시간이 소요됩니다. </li>
  </ul>
</details>

<details>
  <summary>Array와 Linked List를 비교해서 설명해주세요</summary>
  <ul>
    <li> 배열은 메모리상에서 연속적으로 데이터를 저장하는 자료구조입니다. 링크드리스트는 메모리상에서 연속적이지 않지만 다음 원소의 주소값을 저장해둠으로서 논리적인 연속성을 가집니다. 그래서 각 operation의 시간 복잡도가 다릅니다. 데이터 조회의 경우 배열은 O(1), 리스트는 O(n)을 가집니다. 삽입/삭제의 경우 배열은 O(n), 리스트는 O(1)을 가집니다. 따라서 얼마만큼의 데이터를 저장할지 예측이 되고 조회가 많다면 배열을, 불확실하고 삽입과 삭제가 많다면 링크드리스트를 사용하는것이 좋습니다.</li>
  </ul>
</details>

<details>
  <summary>Array와 Linked List의 memory allocation은 언제 일어나며, 메모리의 어느 영역을 할당 받나요?</summary>
  <ul>
    <li> Array는 컴파일 단계에서 메모리 할당이 일어납니다. 이를 static allocation이라고 합니다. 이 경우 스택 메모리 영역에 할당됩니다. 링크드리스트는 runtime단계에서 노드가 추가될 때마다 할당이 일어나게 됩니다.  이 경우 힙 영역에 할당되게 됩니다. </li>
  </ul>
</details>

## Queue와 Stack

<details>
  <summary>Queue는 무슨 자료구조인가요?</summary>
  <ul>
    <li> Queue는 선입선출의 자료구조입니다. 시간 복잡도는 enqueue와 dequeue O(1) 입니다. 활용 예시는 캐시 구현, 프로세스 관리, BFS가 있습니다. </li>
  </ul>
</details>

<details>
  <summary>Array-Base 와 List-Base의 경우 어떤 차이가 있나요?</summary>
  <ul>
    <li> Array-Based 큐는 순환큐로 구현하는 것이 일반적입니다. 이는 메모리를 효율적으로 활용하기 위함입니다. 또한 enqueue가 계속 일어나면 고정된 사이즈를 넘어서기 때문에 Dynamic Array와 같은 방법으로 리사이즈를 해줘야합니다. 그럼에도 enqueue의 시간복잡도는 amortized O(1)을 유지할 수 있습니다.</li>
    <li> List-Base 큐는 보통 single-linked list로 구현합니다. append는 single-linked list에 append 하는것으로 구현되며 O(1)의 시간복잡도를 가집니다. dequeue의 경우에도 맨 앞 원소를 삭제하면 되기 때문에 O(1)의 시간복잡도를 가집니다. </li>
    <li> 두가지 모두 시간복잡도는 O(1)의 시간을 가지게됩니다. 배열 기반의 경우 전반적인 퍼포먼스는 좋지만 최악의 경우 리사이즈로 인해 느릴 수 있습니다. 리스트 기반의 경우 원소 추가/삽입 시 메모리 할당이 일어나므로 전반적으로 런타임이 느릴 수 있습니다.</li>
  </ul>
</details>

<details>
  <summary>Stack은 무슨 자료구조인가요?</summary>
  <ul>
    <li> Stack은 후입선출의 자료구조입니다. 시간 복잡도는 push, pop 모두 O(1) 이며 활용 예제로는 후위 표기법 연산, 괄호 유효성 검사, 웹 브라우저 방문기록, DFS 등이 있습니다. </li>
  </ul>
</details>

<details>
  <summary>Stack 두개를 이용해 큐를 구현해주세요. 그것의 시간복잡도는 어떻게 되나요?</summary>
  <ul>
    <li> qeueue의 enqueue를 구현할 때 첫번째 스택을 이용하고, dequeue를 구현할 때 두번째 스택을 사용합니다. </li>
    <li> enqueue의 경우 O(1)의 시간복잡도를 가집니다. dequeue는 최악의 경우에 2번째 스택이 비어있을 경우 1번째 스택에 있는 모든 데이터를 pop 하고 push 해줘야 하기 때문에 2*N 이 되어 O(n)의 시간이 소요됩니다. 만약 비어있지 않다면 pop만 해주면 되서 O(1)의 시간이 소요됩니다. 이를 종합했을때 amortized O(1)의 시간복잡도를 가진다고 할 수 있습니다. </li>
  </ul>
</details>

<details>
  <summary>큐 두개를 이용해 스택을 구현해주세요. 그것의 시간복잡도는 어떻게 되나요?</summary>
  <ul>
    <li> 먼저 데이터를 넣을때는 첫번째 큐에 enqueue를 통해 넣습니다.  pop을 할 경우에는 첫번째 큐에 원소가 1개 이하로 남을 떄까지 모든 데이터를 2번째 큐에 옮겨담습니다. 한개 남은 데이터를 반환하고, 첫번째 큐와 두번째 큐의 이름을 바꿉니다. </li>
    <li> push의 경우 O(1)의 시간이 걸리고, pop의 경우 n-1개의 원소를 2번째 큐로 옮겨야 하기 때문에 O(n)의 시간이 소요됩니다. </li>
  </ul>
</details>

<details>
  <summary>Queue와 Priority Queue의 차이점에 대해 설명해주세요</summary>
  <ul>
    <li> Queue는 시간 순서상 먼저 들어간 데이터가 먼저 나오는 구조입니다. 우선순위 큐는 시간과 상관없이 우선순위가 높은 원소가 먼저 반환되는 구조입니다. Queue의 시간복잡도는 enqueue dequeue 모두O(1)이며 우선순위 큐는 엔큐와 디큐 모두 O(log N) 입니다. </li>
  </ul>
</details>

<details>
  <summary>Heap이 뭔가요?</summary>
  <ul>
    <li> Heap은 우선순위 큐를 구현하기 위해 만들어진 자료구조입니다. 완전 이진 트리 구조이며, 종류에는 최대 힙, 최소 힙이 있습니다. 최대 힙은 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같으며, 최소 힙은 부모 노드의 값이 자신노드의 키 값보다 작거나 같은 완전 이진 트리입니다. 힙은 트리임에도 배열을 이용해 구현됩니다. 그 이유는 새로운 노드를 힙의 마지막에 추가해야하는데 이 과정을 배열로 구현해야 수월해지기 때문입니다. 또한 편의를 위해 0번째 인덱스는 사용하지 않습니다.  </li>
  </ul>
</details>

<details>
  <summary>우선순위큐는 왜 힙으로 구현된나요? BST로 하면 안되나요?</summary>
  <ul>
    <li> 우선순위 큐는 최소값 또는 최대값을 가장 빠르게 가져오는 것이 핵심인데, 이 연산은 힙에서 O(1)로 처리할 수 있어 매우 유리합니다. BST로도 우선순위 큐를 구현할 수 있지만 BST에서는 최솟값/최댓값 조회가 O(log N)이며, 구조가 포인터 기반이라 메모리 면에서 비효율적이고 캐시 친화성도 떨어집니다. 반면 힙은 배열 기반의 완전 이진 트리라서 메모리 효율이 좋고 삽입/삭제도 O(log N)에 안정적으로 보장되며, 구현도 단순합니다. 그래서 Java, C++, Python 등 대부분 언어에서 우선순위 큐는 이진 힙으로 구현됩니다.</li>
  </ul>
</details>


## Hash Table & BST(Binary Search Tree)

<details>
  <summary>BST는 뭔가요?</summary>
  <ul>
    <li> 이진 탐색트리는 정렬된 트리입니다. 어느 노드를 선택하든 그 왼쪽에는 해당 노드보다 작은 값들을 가진 노드들로 이루어져있고, 오른쪽에는 해당 노드보다 큰 노드들이 위치해있습니다. 검색과 저장, 삭제 모두 O(log N)의 복잡도를 가지며 모든 노드가 한쪽으로 치우쳐 있는 최악의 경우에는 O(N)입니다.  </li>
  </ul>
</details>

<details>
  <summary>BST의 worst case는 어떻게 해결하나요?</summary>
  <ul>
    <li> 자가 균형 이진 탐색 트리(self-balancing bst)는 알고리즘으로 이진트리의 균형이 잘 맞도록 유지하여 높이를 가능한 낮게 유지합니다. 대표적으로 AVL 트리와 Red-black tree가 있습니다. JAVA에서는 해쉬맵의 sperate chaning으로서 링크드리스트와 레드블랙트리를 병행하여 저장합니다. </li>
  </ul>
</details>

<details>
  <summary>Hash Table은 어떤 자료구조 인가요?</summary>
  <ul>
    <li> Hash table은 효율적인 탐색(빠른 탐색)을 위한 자료구조로서 key-value 쌍의 데이터를 받습니다. 해쉬 함수 h에 키값을 입력으로 얻은 해쉬값 h(k)를 위치로 지정하여 key-value 데이터 쌍을 저장합니다. 저장, 삭제, 검색의 시간복잡도는 모두 O(1)입니다. 키 밸류 데이터를 저장할 수 있는 각각의 공간을 bucket 또는 slot이라고 합니다.</li>
    <li> 하지만 collision으로 인해 최악의 경우 O(n)이 될 수 있습니다. 공간 효율성 또한 떨어집니다. 미리 slot이나 bucket을 할당받아 놓아야 하며 저장공간이 부족하거나 채워지지 않은 경우가 발생할 수 있습니다.</li>
  </ul>
</details>

<details>
  <summary>좋은 해시 함수의 기준은 뭔가요?</summary>
  <ul>
    <li> 좋은 해시 함수란 입력 데이터를 전체 버킷 공간에 고르게 분산시키고, 충돌을 최소화하며, 계산이 빠르며, 작은 입력 변화에도 해시값이 크게 달라지는 함수입니다</li>
  </ul>
</details>

<details>
  <summary>Collision이 뭔가요?</summary>
  <ul>
    <li> Collision이란 서로다른 키값의 해시값이 똑같을때를 말합니다. 중복되는 값이 아니지만 해시값이 중복하는 상황에 발생합니다. collision이 최대한 적게 나도록 해시 함수를 잘 설계해야 하며, 어쩔 수 없이 발생하는 경우 seperate chaining 또는 open address 기법을 사용해 해결합니다. </li>
  </ul>
</details>

<details>
  <summary>해시 테이블에서 Collision이 발생 했을때 어떻게 되고 해결방법엔 뭐가 있나요?</summary>
  <ul>
    <li> 대표적으로 두가지 방법이 있습니다. open address 방식은 collision이 발생하면 미리 정해진 규칙에 따라 비어있는 slot을 찾습니다. 빈 슬롯을 찾는 방법에 따라 크게 Linear Probing, Quadratic Probing, Double Hashing으로 나뉩니다. seperate chaning은 링크드 리스트를 이용합니다. collision이 일어나면 링크드 리스트에 slot을 추가해 데이터를 저장합니다.</li>
  </ul>
</details>

<details>
  <summary>Open addressing 방법에 대해 설명해주세요</summary>
  <ul>
    <li> open address 방식은 collision이 발생하면 미리 정해진 규칙에 따라 비어있는 slot을 찾습니다. 빈 슬롯을 찾는 방법에 따라 크게 Linear Probing, Quadratic Probing, Double Hashing으로 나뉩니다. </li>
    <li> 선형 조사법(Linear Probing)은 충돌이 발생한 해시값으로부터 일정한 값만큼 건너뛰어 비어있는 slot에 데이터를 저장합니다. 이차 조사법은 (quandratic probing) 제곱수로 건너뛰어 빈 슬롯을 찾습니다. 충돌이 여러번 발생하면 특정 영역에 데이터가 집중적으로 모이는 클러스터링 현상이 발생는 단점이 있으며 클러스터링 현상은 평균 탐색 시간을 높히게 됩니다.</li>
    <li> Double Hashing 이중해시는 선형 조사법이나 이차 조사법에서 생기는 클러스터링 현상을 해결하기 위해 2개의 해싱 함수를 사용하는 방법입니다. 하나는 최초의 해시값을 얻을때 사용되고 하나는 충돌이 발생했을 때 탐사 이동폭을 얻기 위해 사용됩니다. </li>
  </ul>
</details>

<details>
  <summary>Seperate Chaning에 대해 설명해주세요</summary>
  <ul>
    <li> seperate chaning은 링크드 리스트를 이용합니다. collision이 일어나면 링크드 리스트에 slot을 추가해 데이터를 저장합니다. </li>
    <li> 삽입: 서로 다른 두 키가 같은 해시값을 가지게 되면 링크드 리스트를 추가해 키 밸류를 저장합니다. 시간 복잡도는 O(1)입니다. </li>
    <li> 검색: 기본적으로 O(1) 이지만 최악의 경우 O(n)을 가집니다. </li>
    <li> 삭제를 하기 위해서 기본적으로 검색을 해야해서 같은 시간복잡도를 가져 O(1)이지만 최악의 경우 O(N)입니다. </li>
    <li> 기본적으로 링크드 리스트를 이용해 구현을 하지만 충돌이 많아 링크드 리스트의 길이가 길어지면 BST를 이용해 저장을 하는 경우도 있습니다. BST는 최악의 경우 시간복잡도를 O(n)에서 O(log N)으로 낮춰줍니다. </li>
  </ul>
</details>

<details>
  <summary>Direct-address Table은 어떤 자료구조 인가요?</summary>
  <ul>
    <li> 직접 주소 table은 key값으로 k를 가지는 원소는 k인덱스에 저장하는 방식입니다. 불필요한 공간이 낭비되며 다양한 자료형을 키로 사용할 수 없게됩니다. </li>
  </ul>
</details>

<details>
  <summary>Load Factor에 대해 설명해 주세요.</summary>
  <ul>
    <li> Load Factor는 해시 테이블이 얼마나 채워졌는지를 나타내는 지표로, size를 capacity로 나눈 값입니다. 이 값이 높을수록 메모리는 효율적이지만 충돌이 많이 발생해 성능이 떨어지고, 너무 낮으면 메모리를 낭비하게 됩니다. </li>
  </ul>
</details>

<details>
  <summary>다른 자료구조와 비교하여, 해시 테이블은 멀티스레드 환경에서 심각한 수준의 Race Condition 문제에 빠질 위험이 있습니다. 성능 감소를 최소화 한 채로 해당 문제를 해결할 수 있는 방법을 설계해 보세요.</summary>
  <ul>
    <li>해시 테이블은 내부적으로 같은 버킷의 체인이나 트리를 수정할 때 Race Condition이 쉽게 발생하기 때문에 멀티스레드 환경에서 주의해야 합니다. 가장 단순한 방법은 전체 맵에 대한 전역 락을 거는 것이지만, 이렇게 되면 모든 연산이 직렬화되어 성능이 크게 떨어집니다. 성능 저하를 최소화하려면 락을 세분화하는 방식이 필요합니다.예를 들어 버킷이나 세그먼트 단위로 락을 나누거나,Java 8 ConcurrentHashMap처럼 빈 버킷 삽입은 CAS로 처리하고, 충돌이 있을 때만 해당 버킷 체인에 대해 짧게 synchronized를 사용하는 전략을 쓸 수 있습니다. 읽기가 훨씬 많은 경우에는 Copy-On-Write나 immutable 구조를 활용해 읽기는 락 없이 처리하고, 쓰기 시에만 새 구조로 교체하는 방식도 고려할 수 있습니다 </li>
  </ul>
</details>

<details>
  <summary>Map과 해시맵의 차이는?</summary>
  <ul>
    <li> map 컨테이너는 BST를 사용하다가 최근에 레드블랙 트리를 사용하는중입니다. 키값을 이용해 트리를 탐색하는 방식이며 따라서 데이터의 접근,삽입,삭제 모두 O(log N)의 복잡도를 가집니다. 해시맵은 해시함수를 활용해 O(1)로 접근이 가능합니다. </li>
  </ul>
</details>

<details>
  <summary>트리맵은 어떤 자료구조 기반인가요?</summary>
  <ul>
    <li> 트리맵은 레드 블랙 트리 기반의 정렬된 맵입니다. 따라서 키 기준으로 자동 정렬되며 탐색 삽입 삭제 모두 O(log N)입니다. 순서 유지가 필요할 때 해쉬맵 대신 사용합니다. </li>
  </ul>
</details>

<details>
  <summary>트리는 어떤 자료구조인가요?</summary>
  <ul>
    <li> 트리는 노드로 이루어진 자료 구조입니다. 트리는 하나의 루트 노드를 가지며 루트 노드는 0개 이상의 자식 노드를 가지고 있으며, 그 자식노드 또한 0개 이상의 자식을 가지고 있습니다. 노드와 노드를 연결하는 간선들로 구성되어 있습니다. 트리에는 사이클이 존재할 수 없고 노드들은 특정 순서로 나열될 수도 있고 아닐수도 있습니다. 비선형 자료구조로 계층적 관계를 표현하며(조직도 등) 그래프의 한 종류입니다. 사이클이 없는 하나의 연결 그래프 또는 방향성이 있는 비순환 그래프의 일종입니다.</li>
    <li> 트리는 계층 모델이며 최소 연결 트리라고도 불립니다. 트리는 루프나 사이클이 존재하지 않으며 노드가 n개인 트리는 항상 N-1개의 간선을 가집니다. 두개의 정점 사이에 반드시 하나의 경로만을 가지며 한개의 루트 노드만이 존재합니다. 순회는 Pre-order, in-order, post-order로 이루어집니다. 트리는 이진트리, 이진 탐색 트리, 균형(AVL, RED-black)트리, 이진 힙 등이 있습니다.</li>
  </ul>
</details>

<details>
  <summary>AVL트리와 Red-Black 트리</summary>
  <ul>
    <li> AVL 트리는 노드마다 balance factor(균형 인수, 왼쪽 높이 - 오른쪽 높이)를 유지하며 -1, 0, +1만 허용하는 매우 엄격하게 균형 잡힌 트리입니다. 이 때문에 탐색 속도는 모든 balanced BST 중에 가장 빠르지만, 삽입과 삭제 때 회전이 많이 발생해 성능이 떨어집니다. 반면 Red-Black 트리는 노드를 빨강과 검정으로 색칠해서 느슨한 균형을 유지하는 트리입니다. 루트에서 리프까지 모든 경로의 블랙 노드 수는 동일함을 유지하는 규칙으로 트리 높이를 보장합니다. 삽입/삭제 시 회전이 훨씬 적어 실제 라이브러리 구현에서 대부분 많이 사용합니다. </li>
    <li> 탐색이 매우 빠른게 중요하다면 AVL, 삽입/삭제가 많은 범용 구현은 레드-블랙 트리를 사용합니다.</li>
  </ul>
</details>

## 기타

<details>
  <summary>TreeSet vs HashSet vs LinkedHashSet</summary>
  <ul>
    <li> HashSet은 정렬이 없고 가장 빠르며 내부 구조가 해쉬맵으로 되어있습니다. </li>
    <li> LinkedHashSet은 삽입 순서가 유지되는 해쉬 셋이며, 해쉬셋 + 이중연결 리스트가 추가 된 구조입니다. 순서있는 set를 보장할 수 있습니다.</li>
    <li> TreeSet은 정렬을 보장하고 내부 구조가 레드블랙 트리로 된 구조입니다. 항상 정렬된 상태로 유지되며 탐색, 삽입, 삭제 모두 O(logN)의 시간으로 수행합니다.</li>
  </ul>
</details>

<details>
  <summary>TreeSet vs PriorityQueue</summary>
  <ul>
    <li> HashSet은 정렬이 없고 가장 빠르며 내부 구조가 해쉬맵으로 되어있습니다. </li>
    <li> LinkedHashSet은 삽입 순서가 유지되는 해쉬 셋이며, 해쉬셋 + 이중연결 리스트가 추가 된 구조입니다. 순서있는 set를 보장할 수 있습니다.</li>
    <li> TreeSet은 정렬을 보장하고 내부 구조가 레드블랙 트리로 된 구조입니다. 항상 정렬된 상태로 유지되며 탐색, 삽입, 삭제 모두 O(logN)의 시간으로 수행합니다.</li>
  </ul>
</details>

<details>
  <summary>LRU Cache는 어떤 자료구조로 구현하나요?</summary>
  <ul>
    <li> TreeSet은 Red-Black Tree 기반으로 전체 요소가 정렬된 상태를 유지하는 Set입니다. 삽입·삭제는 O(log N)이며 정렬된 순회가 가능합니다. PriorityQueue는 Binary Heap 기반으로 최솟값 또는 최댓값 하나만 가장 빠르게 가져올 수 있도록 최적화된 자료구조입니다. peek은 O(1)이고 pop도 O(log N)이지만 전체가 정렬 상태는 아닙니다. 즉, TreeSet은 "정렬된 전체 집합"이 필요할 때, PriorityQueue는 "가장 우선순위 높은 요소를 계속 뽑는 작업"이 필요할 때 사용합니다.</li>
  </ul>
</details>

<details>
  <summary>Heap이 왜 필요한가요?</summary>
  <ul>
    <li> 최소/최대 값을 빠르게 추출해야 할 때 Heap이 유리합니다. 루트 노드가 최소값이기 때문에 O(1)에 접근할 수 있고, 삽입/삭제도 log(N)으로 빠르기 때문입니다. 우선순위 큐, 스케줄러 등에 필수적입니다.</li>
  </ul>
</details>

<details>
  <summary>그래프 탐색에서 방문 배열이 필요한 이유는 뭔가요?</summary>
  <ul>
    <li> 방문 배열은 중복 방문을 막고, 사이클이 있는 그래프에서 무한 루프를 방지합니다. 또한 시간 복잡도를 O(V + E)로 유지하기 위해 불필요한 탐색을 줄이는 역할을 합니다.</li>
  </ul>
</details>

<details>
  <summary>트리와 그래프를 비교해서 설명해주세요</summary>
  <ul>
    <li> 트리는 사이클이 없는 연결 그래프이며, 노드 수 N이면 간선은 항상 n-1개입니다. 반면 그래프는 방향/무방향 여부, 사이클 여부 등 훨씬 복잡한 구조를 표현할 수 있습니다.</li>
  </ul>
</details>

<details>
  <summary>해시 충돌이 많으면 어떻게 되나요?</summary>
  <ul>
    <li> 충돌이 많아지면 특정 버킷에 데이터가 몰리기 때문에 linkedlist 길이가 늘어나 O(n)까지 떨어집니다. 자바는 이 문제를 해결하기 위해 길이가 8 이상이면 레드블랙 트리로 변환해 O(log N)을 유지합니다.</li>
  </ul>
</details>

<details>
  <summary>배열 기반으로 큐와 스택을 구현했을 때 문제점</summary>
  <ul>
    <li> 스택을 구현할 경우 공간이 꽉 차면 오버플로우가 발생해 확장이 필요합니다. 또한 고정 크기로 선언하면 메모리 낭비가 발생할 수 있습니다. 큐도 삭제할 때 앞 공간이 비어도 rear만 증가해 메모리 낭비가 발생하며, 그래서 보통 원형 큐로 구현합니다.</li>
  </ul>
</details>

<details>
  <summary>그래프에 대해 설명해주세요</summary>
  <ul>
    <li> 그래프는 노드와 그 노드를 연결하는 간선을 하나로 모아 놓은 자료구조입니다. 연결되어 있는 개체 간의 관계를 표현할 수 있는 자료구조입니다. 그래프는 여러개의 고립된 부분 그래프로 구성될 수 있습니다.</li>
    <li> 그래프는 네트워크 모델이며, 2개 이상의 경로가 가능합니다. 루트 노드나 부모-자식 관계라는 개념이 없으며 순회는 DFS나 BFS로 이루어집니다. 순환 또는 비순환이며 방향/무방향 그래프가 있습니다.</li>
  </ul>
</details>

<details>
  <summary>그래프 저장 방식엔 뭐가있나요?</summary>
  <ul>
    <li> 인접 리스트는 메모리가 적게 들고 대부분 상황에서 효율적입니다. 인접 행렬은 O(1)로 연결 여부를 바로 알 수 있지만 메모리를 많이 사용합니다. 노드 수가 매우 많다면 인접 리스트를 사용합니다.</li>
  </ul>
</details>

<details>
  <summary>ConcurrentHashMap이 빠른 이유가 뭔가요?</summary>
  <ul>
    <li> ConcurrentHashMap은 전체 맵에 락을 걸지 않고, 내부적으로 락을 여러 세그먼트로 분리해 락 contention을 줄입니다. 또한 CAS연산을 활용해 고성능을 보장합니다.</li>
    <li> 기존에 Collections.synchronizedMap은 조회와 수정 모두에 대해 전체 맵에 하나의 락을 거는 방식이라, 동시성이 높아질수록 성능이 급격히 떨어집니다. 반면 ConcurrentHashMap은 버킷 단위로 락을 나누거나, Java 8부터는 CAS와 volatile을 활용해 읽기 연산은 거의 락 없이, 쓰기 연산만 최소 범위에 락을 거는 방식으로 구현되어 있습니다. 그래서 여러 쓰레드가 동시에 서로 다른 키에 접근할 때 충돌 없이 병렬로 처리할 수 있고, 해시 버킷이 너무 길어질 경우에는 Red-Black Tree로 변환해 최악의 경우에도 O(log N) 정도의 성능을 유지합니다. 이런 점 때문에 고동시성 환경에서 synchronizedMap보다 훨씬 빠르게 동작합니다.</li>
  </ul>
</details>

<details>
  <summary>Load Factor가 뭔가요?</summary>
  <ul>
    <li> HashMap에서 load factor는 현재 저장된 요소 수를 버킷 크기로 나눈 값으로, “해시 테이블이 얼마나 차 있는지”를 나타내는 지표입니다. HashMap은 capacity * loadFactor 가 threshold가 되는데, 이 threshold를 넘으면 내부 배열을 두 배로 늘리고, 모든 엔트리를 새 배열로 재해싱합니다. load factor를 너무 낮게 잡으면 메모리를 많이 쓰고 자주 리사이징하지만 충돌은 적고, 너무 높게 잡으면 리사이징은 줄어들지만 충돌이 증가해서 성능이 떨어집니다. 그래서 자바에서는 보통 0.75를 기본값으로 사용해서 성능과 메모리 사용 간의 균형을 맞추고 있습니다.</li>
  </ul>
</details>

<details>
  <summary>Trie는 뭔가요?</summary>
  <ul>
    <li> Trie는 문자열 검색에 특화된 트리 기반 자료구조로, 각 노드가 문자 하나를 나타내고 루트부터의 경로가 하나의 문자열이 됩니다. 어떤 문자열이 존재하는지 확인할 때, 문자열 길이를 L이라고 하면 해시 맵처럼 전체 문자열을 통째로 해싱하는 게 아니라, 문자를 하나씩 내려가며 탐색하기 때문에 시간복잡도가 O(L)입니다. 특히 많은 문자열이 공통 접두어를 공유할 때 메모리도 절약되고, “이 prefix로 시작하는 모든 단어를 찾아라” 같은 요구사항에 매우 유리해서 자동완성, 검색어 추천, 금칙어 필터링 같은 기능에 많이 사용됩니다.</li>
  </ul>
</details>

# 알고리즘
<details>
  <summary>시간 복잡도와 공간 복잡도에 대해 설명해주세요?</summary>
  <ul>
    <li>시간 복잡도는 알고리즘이 입력 크기 n에 따라 얼마나 많은 연산을 수행하는지를 나타내는 척도입니다. 상수 시간이나 세부 연산 횟수보다는 입력 크기가 커졌을 때 성능이 어떻게 증가하는지를 Big-O 표기법으로 표현합니다. </li>
    <li>공간 복잡도는 알고리즘이 동작하기 위해 추가로 필요한 메모리 양을 의미합니다. 반복문은 O(1) 공간만 필요하지만, 재귀나 Merge Sort처럼 추가 메모리를 사용하는 알고리즘은 O(n)의 공간 복잡도를 갖습니다./li>
  </ul>
</details>

<details>
  <summary>Big-O, Big-Theta, Big-Omega 에 대해 설명해 주세요.</summary>
  <ul>
    <li>Big-O, Big-Theta, Big-Omega는 모두 알고리즘의 시간 복잡도를 표현하는 표기법입니다. Big-O는 상한을 나타내는 개념으로, 알고리즘이 최대로 오래 걸릴 수 있는 시간, 즉 최악의 경우를 표현합니다. 예를 들어 QuickSort는 최악의 경우 O(n²)입니다. 반대로 Big-Omega는 하한, 즉 최선의 경우 알고리즘이 최소한으로 걸리는 시간을 의미합니다. Big-Theta는 상한과 하한이 같은, 정확한 시간 복잡도를 의미하며 Merge Sort처럼 전체 복잡도가 항상 n log n 형태일 때 Θ(n log n)으로 표현합니다./li>
  </ul>
</details>

<details>
  <summary>왜 Big-O를 많이 사용할까요?</summary>
  <ul>
    <li>Big-O는 최악의 경우의 상한을 나타내기 때문에, 알고리즘의 안전한 성능 예측이 가능하고 입력이 커질 때 성능 보장을 하는 데 가장 유용하기 때문입니다./li>
  </ul>
</details>

<details>
  <summary>O(1)은 O(N^2) 보다 무조건적으로 빠른가요?</summary>
  <ul>
    <li>O(1)이 O(N²)보다 무조건 빠른 것은 아닙니다. Big-O는 입력 크기가 충분히 커졌을 때의 증가율만 표현할 뿐, 실제 실행 시간에 큰 영향을 미치는 상수 시간이나 메모리 접근 패턴은 숨기기 때문입니다. 예를 들어 O(1)이지만 내부적으로 매우 큰 상수 연산을 수행할 수 있고, O(N²)이라도 입력이 작거나 캐시 친화적인 구조라면 실제로 더 빠를 수 있습니다./li>
  </ul>
</details>

<details>
  <summary>Quick Sort와 Merge Sort를 비교해서 설명해주세요</summary>
  <ul>
    <li>QuickSort는 pivot을 기준으로 제자리에서 분할하는 in-place 정렬이어서 공간 복잡도가 O(log n)으로 매우 작고, 캐시 효율이 좋아 평균적으로 매우 빠릅니다. 하지만 pivot을 잘못 선택하면 최악의 경우 O(n²)이 되는 단점이 있습니다. 반면 MergeSort는 항상 O(n log n)의 시간 복잡도를 보장하고 stable한 정렬이며, 병렬화에도 적합합니다. 대신 merge 과정에서 O(n)의 추가 메모리가 필요하고 캐시 친밀도가 낮아 실제 연산은 QuickSort보다 느릴 수 있습니다. 그래서 실무에서는 primitive 타입에는 QuickSort, 객체 정렬에는 안정성과 worst-case 보장을 위해 MergeSort 계열(TimSort)이 사용됩니다./li>
  </ul>
</details>

<details>
  <summary>Quick Sort에서 O(N^2)이 걸리는 예시를 들고, 이를 개선할 수 있는 방법에 대해 설명해 주세요.</summary>
  <ul>
    <li>QuickSort는 pivot을 기준으로 제자리에서 분할하는 in-place 정렬이어서 공간 복잡도가 O(log n)으로 매우 작고, 캐시 효율이 좋아 평균적으로 매우 빠릅니다. 하지만 pivot을 잘못 선택하면 최악의 경우 O(n²)이 되는 단점이 있습니다. 반면 MergeSort는 항상 O(n log n)의 시간 복잡도를 보장하고 stable한 정렬이며, 병렬화에도 적합합니다. 대신 merge 과정에서 O(n)의 추가 메모리가 필요하고 캐시 친밀도가 낮아 실제 연산은 QuickSort보다 느릴 수 있습니다. 그래서 실무에서는 primitive 타입에는 QuickSort, 객체 정렬에는 안정성과 worst-case 보장을 위해 MergeSort 계열(TimSort)이 사용됩니다.</li>
  </ul>
</details>

<details>
  <summary>Stable Sort가 무엇이고, 어떤 정렬 알고리즘이 Stable 한지 설명해 주세요.</summary>
  <ul>
    <li>Stable Sort는 같은 값을 가진 요소들이 정렬 후에도 원래의 상대적 순서를 유지하는 정렬 방식입니다.예를 들어 동일 점수를 가진 학생을 정렬했을 때, 입력 순서를 그대로 유지해야 한다면 Stable Sort가 필요합니다.대표적으로 Merge Sort, Insertion Sort, Bubble Sort, Counting Sort, Radix Sort, 그리고 Java와 Python에서 사용되는 TimSort가 안정 정렬입니다. 반면 QuickSort, HeapSort, SelectionSort는 swap 과정에서 동일 값의 원소 순서를 바꿀 수 있어 unstable 합니다. 안정 정렬은 multi-key 정렬이나 실무의 데이터 처리에서 원본 순서를 보존해야 할 때 매우 중요합니다.</li>
  </ul>
</details>

<details>
  <summary>Merge Sort를 재귀를 사용하지 않고 구현할 수 있을까요?</summary>
  <ul>
    <li>Merge Sort는 재귀 없이도 구현 가능합니다. 보통 이를 Bottom-Up Merge Sort 라고 부르며,
배열을 1개 단위로 정렬된 구간으로 보고 그 길이를 1 → 2 → 4 → 8처럼 두 배씩 늘려가며 반복적으로 merge합니다. Merge Sort의 본질은 “병합”이기 때문에 재귀는 필수가 아니고, 반복문만으로도 동일한 O(N log N) 성능을 낼 수 있습니다. 이 방식은 재귀 스택을 사용하지 않아 메모리 사용량이 줄고 대규모 데이터에서도 안정적으로 동작한다는 장점이 있습니다.</li>
  </ul>
</details>

<details>
  <summary>Bubble, Selection, Insertion Sort의 속도를 비교해 주세요.</summary>
  <ul>
    <li>Bubble Sort는 비교와 swap을 반복해서 수행하기 때문에 가장 비효율적이고 가장 느린 편입니다.</li>
    <li>Selection Sort는 비교 횟수는 O(N²)로 동일하지만 한 패스에 swap을 딱 한 번만 수행하므로 Bubble보다는 조금 효율적입니다. 다만 입력 상태와 상관없이 항상 동일한 시간이 걸립니다.</li>
    <li>Insertion Sort는 이미 정렬된 부분을 활용하기 때문에 거의 정렬된 데이터에서는 O(N)에 가까운 속도를 보여주고, 메모리 접근 패턴도 연속적이라 실제 실행 속도는 세 알고리즘 중 가장 빠릅니다. 그래서 TimSort 같은 실무 정렬 알고리즘에서도 작은 구간에서는 Insertion Sort를 활용합니다.</li>
  </ul>
</details>

<details>
  <summary>값이 거의 정렬되어 있거나, 아예 정렬되어 있다면, 위 세 알고리즘의 성능 비교 결과는 달라질까요?</summary>
  <ul>
    <li>세 알고리즘 모두 평균적으로 O(N²)이지만, 입력이 정렬되어 있거나 거의 정렬된 상태라면 성능 차이가 크게 달라집니다. Insertion Sort는 이전 구간이 항상 정렬되어 있기 때문에 정렬된 배열에서는 비교 한 번으로 바로 넘어가며 O(N)까지 빨라집니다. 거의 정렬된 상태에서도 이동량이 적어 매우 빠릅니다.</li>
    <li>Bubble Sort는 정렬된 배열에서는 한 번의 패스에서 swap이 없기 때문에 O(N)에 종료되지만, 조금만 어긋나 있어도 여전히 O(N²)에 가깝습니다.</li>
    <li>반면 Selection Sort는 항상 전체 배열을 스캔하며 최소값을 찾기 때문에 정렬 여부와 상관없이 비교 횟수가 동일해 O(N²) 성능이 유지됩니다.</li>
  </ul>
</details>

<details>
  <summary>자바는 어떤 알고리즘을 가지고 정렬을 수행하나요?</summary>
  <ul>
    <li>자바는 하나의 정렬 알고리즘이 아니라 배열 타입에 따라 서로 다른 정렬 알고리즘을 사용합니다. 객체 배열(Object[])은 TimSort를 사용하는데, TimSort는 Merge Sort와 Insertion Sort를 조합한 안정 정렬로 실제 데이터가 부분적으로 정렬되어 있을 때 매우 빠르고 최악에서도 O(N log N) 성능을 보장합니다. 반면 primitive 배열(int[], long[] 등)은 Dual-Pivot QuickSort를 사용합니다. 이 방식은 두 개의 pivot으로 배열을 세 부분으로 나누기 때문에 평균 성능이 매우 뛰어나고 메모리 사용도 적습니다. 또한 Java 8부터는 멀티코어를 활용하는 Arrays.parallelSort()도 제공됩니다. 그래서 자바는 데이터 타입과 상황에 따라 가장 효율적인 정렬 구현을 선택하고 있습니다.</li>
  </ul>
</details>

<details>
  <summary>메모리 보다 큰 용량의 데이터를 정렬해야한다면 어떻게 하나요?</summary>
  <ul>
    <li>전체를 한 번에 메모리에 올릴 수 없기 때문에 외부 병합 정렬(External Merge Sort) 를 사용합니다. 먼저 메모리에 들어갈 수 있는 크기만큼, 예를 들어 2~3GB씩 데이터를 읽어서 메모리 안에서 일반 정렬 알고리즘으로 정렬한 뒤, 그 정렬된 블록을 디스크에 임시 파일(run)으로 여러 개 저장합니다. 그 다음 단계에서는 이 정렬된 run 파일들을 동시에 조금씩 읽어오면서, 각 run의 현재 원소들을 최소 힙 같은 우선순위 큐에 넣고 가장 작은 값부터 하나씩 뽑아서 최종 결과 파일로 쓰는 k-way merge를 수행합니다. 이렇게 하면 한 번에 메모리에 올라오는 데이터는 항상 제한된 양만 유지되기 때문에 4GB 메모리로도 50GB 데이터를 충분히 정렬할 수 있고, 디스크는 주로 순차 I/O로 사용되어 성능도 안정적으로 나옵니다.</li>
  </ul>
</details>

<details>
  <summary>Thread Safe 한 자료구조가 있을까요? 없다면, 어떻게 Thread Safe 하게 구성할 수 있을까요?</summary>
  <ul>
    <li>thread-Safe한 자료구조는 언어에서 이미 여러 가지를 제공합니다. 예를 들어 Java에서는 ConcurrentHashMap, CopyOnWriteArrayList, ConcurrentLinkedQueue, BlockingQueue 같은 자료구조가 있고, 내부적으로 세그먼트 락, CAS, Copy-On-Write 같은 기법으로 동시성을 보장합니다.</li>
    <li>만약 기본 자료구조가 Thread-Safe하지 않다면 외부에서 synchronized나 ReentrantLock으로 보호하거나, 아예 불변 객체로 만들어 공유하되 변경 시 새 객체로 교체하는 방식, 혹은 ThreadLocal이나 요청 단위로 소유하게 해서 애초에 공유하지 않는 설계(Thread Confinement)를 사용할 수 있습니다. 읽기 위주 시스템에서는 Copy-On-Write 컬렉션을 쓰고, 쓰기와 읽기가 모두 많은 캐시나 맵 구조는 ConcurrentHashMap처럼
세밀한 락 분할이나 CAS를 활용하는 전략이 적합하다고 생각합니다.</li>
  </ul>
</details>

<details>
  <summary>배열의 길이를 미리 알고 있다면 더 빠르게 Thread Safe하게 만들 수 있나요?</summary>
  <ul>
    <li>배열의 길이를 알고 있다면 Thread-safe 구현을 훨씬 효율적으로 만들 수 있습니다. 배열은 인덱스가 고정된 독립 슬롯이기 때문에, 전체 자료구조에 락을 걸 필요가 없고 인덱스별로 락을 분리하거나 AtomicIntegerArray 같은 CAS 기반의 lock-free 구조를 사용할 수 있습니다.</li>
  </ul>
</details>

<details>
  <summary>크루스칼과 프림 알고리즘에 대해 설명해주세요</summary>
  <ul>
    <li>크루스칼과 프림은 MST를 만드는 대표적인 두 알고리즘입니다. 크루스칼은 간선을 중심으로 동작하며, 모든 간선을 가중치 기준으로 정렬한 뒤 사이클이 생기지 않도록 Union-Find로 검사하면서 작은 간선부터 선택하는 방식입니다. 시간 복잡도는 간선 정렬로 인해 O(E log E)이며, 희소 그래프에서 특히 효율적입니다. </li>
    <li>반면 프림은 정점을 중심으로 확장하는 방식으로, 하나의 정점에서 시작해서 현재 트리에 가장 적은 비용으로 연결되는 간선을 우선순위 큐로 선택합니다. 인접 리스트 + 힙 사용 시 O(E log V)의 시간 복잡도를 가지며,
밀집 그래프에서 더 좋은 성능을 냅니다. 요약하면 크루스칼은 간선 정렬 + Union-Find 기반의 edge-centric 방식, 프림은 PQ 기반의 vertex-centric 확장 방식이라는 차이가 있습니다.</li>
  </ul>
</details>

<details>
  <summary>정렬 알고리즘 비교표</summary>
  <ul>
    <li><img width="893" height="386" alt="image" src="https://github.com/user-attachments/assets/53daf05b-4c4b-4c11-aa2a-700126ca085c" />
</li>
    <li>시간 복잡도로 보면 단순 정렬인 Bubble, Insertion, Selection은 평균 O(N²)이고, Merge, Quick, Heap, Shell은 평균 O(N log N)입니다. Insertion은 거의 정렬된 경우 O(N)까지 빨라지고, Quick Sort는 평균적으로 가장 빠르지만 pivot 선택이 나쁘면 O(N²)까지 갈 수 있습니다. Merge Sort는 안정 정렬이고 외부 정렬에 적합하지만 공간이 O(N) 필요합니다. Heap Sort는 공간 O(1)로 최악에서도 N log N을 보장하지만 unstable입니다.
</li>
  </ul>
</details>

<details>
  <summary>기타 알고 가야 할것</summary>
  <ul>
    <li>그리디 알고리즘을 통해 해결할수 없는거? -> 동전을 적게 사용해 N원 만들기 문제 (배수가 아닌경우)</li>
    <li>dp - greedy 개념</li>
  </ul>
</details>


