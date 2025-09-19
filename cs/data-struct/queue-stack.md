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
- List-Based Queue
  - 재할당이나 메모리 낭비를 걱정할 필요가 없어짐

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
