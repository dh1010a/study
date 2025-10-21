## Queue & Stack

### Queue
- 선입선출(FIFO) 자료구조
- 시간 복잡도는 enqueue O(1), dequeue O(1)
- 활용 예시로는 Cache 구현, 프로세스 관리, BFS 등이 있음

<img width="800" alt="image" src="https://github.com/user-attachments/assets/5bf42772-5ffc-4071-9b8c-bfe053cfb2e4" />

#### 구현 방식
<img width="800" alt="image" src="https://github.com/user-attachments/assets/207a4d3d-8f7b-4f5f-951e-f53714b8da55" />

- Array-Based Queue
  - enqueue 와 dequeue 과정에서 남는 메모리가 생김. 그래서 메모리의 낭비를 줄이기 위해 주로 Circular queue 형식으로 구현함
  - enqueue가 계속 발생하면 fixed size를 넘어서기 때문에, dynamic array와 같은 방법으로 Array의 사이즈를 확장시켜야함. 그럼에도 시간복잡도는 O(1)을 유지할 수 있음
- List-Based Queue
  - 재할당이나 메모리 낭비를 걱정할 필요가 없어짐
  - enqueue는 단순히 singly-linked list에서 append하는 것으로 구현되고, 이 때 시간 복잡도는 O(1)
  - dequeue는 맨 앞의 원소를 삭제하고 first head를 변경하면 되니까 O(1)의 시간이 소요됨
 
  > - 요약하자면, 두 가지 종류의 자료 구조로 queue를 구현 하더라도 enqueue와 dequeue 모두 O(1)의 시간복잡도를 가짐.
  > - Array-Base의 경우 전반적으로 성능이 더 좋지만, 최악의 경우에는 훨씬 더 느릴 수 있음(resize).
  > - List-Base의 경우 enqueue를 할 때마다 memory 할당을 해야하기 때문에 전반적인 실행 시간이 느릴 수 있음

  

- Circular Queue
<img width="521" height="260" alt="image" src="https://github.com/user-attachments/assets/1a6e9a2d-986e-428b-b704-9f507580a5b8" />

#### 큐의 확장과 활용
Queue에서 확장한 개념으로는 양쪽에서 enqueue와 dequeue가 가능한 Deque(double-end queue)와 시간이 높은 순이 아닌 우선순위대로 정렬되는 Prioiry Queue가 있음

활용 예시로는 하나의 자원을 공유하는 프린터나, CPU task scheduling, Cache구현, 너비우선탐색(BFS) 등이있음

#### 자바에서의 Deque
코딩테스트에서 스택이나 큐 대신 디큐 하나로 해결하는 경우가 많다
<img width="900" height="241" alt="image" src="https://github.com/user-attachments/assets/ebcf8b91-5c2c-46ca-8cdb-1d305b4cbb63" />

<img width="900" height="193" alt="image" src="https://github.com/user-attachments/assets/38c32c4d-117a-4027-b720-0cf791c2d6aa" />

<img width="900" height="199" alt="image" src="https://github.com/user-attachments/assets/38e0ecab-164a-4192-af01-b59debd6699f" />

간단하게 말하면 기존 add()와 offer()은 뒤에서 부터 넣는다고 생각하면 편하다. 그래서 addLast()와 add()가 같고, 원래 큐에서 제공하지 않는 앞에서 넣는것은 addFirst()가 추가로 제공한다

peek()은 앞쪽에 있는 원소를 반환한다. 그렇기떄문에 원래 peek()과 get()은 peekFirst(), getFirst()와 동일하고, 맨 뒤를 보고싶으면 peekLast()를 사용하면 된다.

---

### Queue vs Priority Queue

Queue 자료구조는 시간 순서상 먼저 집어넣은 데이터가 먼저 나오는 FIFO 구조로 저장하는 형식. 이와 다르게 우선순위큐는 들어간 순서와 상관없이 우선순위가 높은 데이터가 먼저 나옴.

Queue의 enqueue와 dequeue 모두 시간 복잡도가 O(1)인 반면, 우선순위 큐는 모두 O(logN)이다.

#### Heap
- 힙은 그 자체로 우선순위 큐의 구현과 일치함
- 완전 이진트리 구조이다. 힙이 되기 위해서는 각 노드에 저장된 값은 child node들에 저장된 값보다 작거나 같아야 하며(min heap), root 노트에 저장된 값이 가장 작은값이 된다.
- 트리는 보통 Linked List로 구현함, 하지만 힙은 트리임에도 불구하고 array를 기반으록 구현해야 한다.
  - 새로운 노드를 힙의 '마지막 위치'에 추가해야 하는데, 이 떄 array 기반으로 구현해야 이 과정이 수월해지기 때문

#### 구현
- 구현의 편의성을 위해 배열의 0번째 인덱스는 사용 X
- 완전 이진트리의 특성을 활용해 배열의 인덱스만으로 부모 자식간의 관계를 정의
  - n번째 노드의 왼쪽 자식 노드 = 2n
  - n번째 노드의 오른쪽 자식 노드 = 2n + 1
  - n번째 노드의 부모 노드 = n / 2
 
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3068424d-c1c2-47bf-826f-58e99bd297e4" />

- Heap push - O(logN)
  - heap tree의 높이는 logN임
  - push()를 했을 때, swap 하는 과정이 최대 logN번 반복되기 때문에 시간복잡도는 O(logn)
- Heap pop - O(logN)
  - pop()을 했을때 스왑하는 과정이 최대 logN번 반복되기 때문에 시간복잡도는 O(logN)
  - 각 노드에 저장된 값은 자식 노드들에 저장된 값보다 크거나 같다(max heap) -> 루트 노드에 저장된 값이 가장 큰 값이 된다


----

### Stack
- 후입선출 LIFO 자료구조
- 시간 복잡도는 push와 pop 모두 O(1)
- 재귀적인 특성이 있음
- 활용 예시로는 후위 표기법 연산, 괄호 유효성 검사, 웹 브라우저 방문 기록(뒤로가기), 깊이 우선 탐색(DFS) 등이 있음

<img width="800" alt="image" src="https://github.com/user-attachments/assets/e10dcd35-7dd8-47ed-86fa-ca410e3ce8c6" />

push와 pop 모두 맨 뒤에 데이터를 추가하고 제거하기만 하면 되서 O(1)의 시간 복잡도를 가진다. push와 pop 모두 stack의 top에 원소를 추가하거나 삭제하는 형식으로 구현됨

----

### Stack 두개를 이용해 Queue 구현하기
큐의 enqueue()를 구현할 때 첫 번째 스택을 사용하고, dequeue()를 구현할 때 두번째 스택을 사용하면 큐를 구현할 수 있음

enqueue()에 사용할 스택을 instack, dequeue()에 사용할 스택을 outstack이라고 부른다면, 
1. enqueue() :: instackdp push()를 해서 데이터를 저장함
2. dequeue() :: 만약 outstack이 비어있다면 instack에서 pop을 하고 outstack에서 push를 하여 instack에 outstack으로 모든 데이터를 옮겨 넣음. 이 결과로 가장 먼저 왔던 데이터는 outstack의 맨 위에 위치하게 되어 pop()을 하면 가장 먼저 왔던 데이터가 추출됨

<img width="800" alt="image" src="https://github.com/user-attachments/assets/bc8a4a9e-815b-4551-a105-569ab602e11e" />

#### 시간 복잡도
두가지 경우를 따져봐야 한다.

1. outstack이 비어있는 경우
  - worst case이다. 이때는 instack에 있는 n개의 데이터를 모두 pop 한 후에 push를 해주어야 해서 2*n의 연산 발생 -> O(n)
2. outstack이 비어있지 않을 경우
  - outstack.pop()만 해주면되어 O(1)의 시간 복잡도를 가짐

이를 종합했을 때, amortized O(1)의 시간복잡도를 갖는다고 할 수 있음

dequeue - O(1), enqueue - O(1)

### Queue 두개를 이용해 Stack 구현하기
push()에 사용할 큐를 1번큐, pop()에 사용할 큐를 2번큐라고 부른다면,
1. push() :: 1번큐에 enqueue() 하여 데이터를 저장
2. pop() ::
   1. 1번큐에 저장된 데이터의 갯수가 1 이하로 남을때 까지 dequeue()를 한 후, 추출된 데이터를 2번큐에 넣는다. 결과적으로 가장 최근에 들어온 데이터를 제외한 모든 데이터는 2번큐로 옮겨짐
   2. 1번큐에 남아 있는 하나의 데이터를 dequeue()해서 가장 최근에 저장된 데이터를 반환함 (LIFO)
   3. 다음에 진행될 pop()을 위와 같은 알고리즘으로 진행하기 위해 1번큐와 2번큐의 이름을 바꾼다

<img width="800" alt="image" src="https://github.com/user-attachments/assets/11cfb5b5-6bf2-4e7f-8641-e810f9f8eb11" />

#### 시간 복잡도
- push(): 1번큐에서 enqueue 한번만 하면 되기 때문에 O(1)을 가짐
- pop(): 1번큐에 저장되어있는 n개의 원소중에 n-1개를 2번큐로 옮겨야 하기 때문에 O(n)이 됨



