# CS 면접을 위한 자료구조 질문리스트

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
    <li> Collision이란 서로다른 키값의 해시값이 똑같을때를 말합니다. 중복되는 값이 아니지만 해시값이 중복하는 상황에 발생합니다. collision이 최대한 적게 나도록 해시 함수를 잘 설계해야 하며, 어쩔 수 없이 발생하는 경우 seperate chaining 또는 open address 기법을 사용해 해결합니다. </li>
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
  <summary>Map과 해시맵의 차이는?</summary>
  <ul>
    <li> map 컨테이너는 BST를 사용하다가 최근에 레드블랙 트리를 사용하는중입니다. 키값을 이용해 트리를 탐색하는 방식이며 따라서 데이터의 접근,삽입,삭제 모두 O(log N)의 복잡도를 가집니다. 해시맵은 해시함수를 활용해 O(1)로 접근이 가능합니다. </li>
  </ul>
</details>

