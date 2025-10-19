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
