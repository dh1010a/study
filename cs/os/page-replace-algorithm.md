# 페이지 교체 알고리즘
- 페이지 부재 발생시 새로운 페이지를 할당해야함
- 현재 할당된 페이지 중 어떤 것을 교체할 지 결정하는 방법

### Page fault (페이지 부재)
- CPU가 무효 비트(invalid bit)로 표시된 page에 엑세스 하는 상황을 page fault라고 함
- CPU가 무효 페이지에 접근하면 주소 변환을 담당하는 하드웨어인 MMU가 **page fault trap을 발생**시키게 되고, 다음과 같은 순서로 page fault를 처리하게 됨
  1. CPU가 페이지 N을 참조
  2. Page table에서 페이지 N이 무효 상태임을 확인
  3. MMU에서 Page fault trap 발생시킴
  4. 디스크에서 페이지 N을 빈 프레임에 적재하고 page table을 업데이트 함 (invalid -> valid)

<img width="800" height="848" alt="image" src="https://github.com/user-attachments/assets/a3a89104-25d8-4467-9517-eedfa4d893db" />


- 가상 메모리는 `요구 페이지 기법`을 통해 필요한 페이지만 메모리에 적재하고 사용하지 않는 부분은 그대로 둠
- 페이지 부재가 발생하면 요청된 페이지를 디스크에서 메모리로 가져옴. 이때 물리적 메모리에 공간이 부족할 수 있다
- 그럴 경우에 메모리에 올라와 있는 페이지를 디스크로 옮겨서 메모리 공간을 확보해야 하는데, 이것을 페이지 교체 (page replacement)라고 함
- 어떤 페이지를 교체할 것이냐를 결정하는 알고리즘이 바로 page 교체 알고리즘
- 여기서 어떤 페이지를 out 시켜야 할지 정해야 함
  - 이 때 out 되는 페이지를 victim 페이지라고 부름
- 교체 알고리즘은 최대한 page fault가 적게 일어나도록 도와줘야 합
- 기왕이면 수정이 되지 않는 페이지를 선택해야 좋음
  - 만약 수정되면 메인 메모리에서 내보낼 때, 하드디스크에서 또 수정을 진행해야 하므로 시간이 오래 걸리기 때문

> 이와 같은 상황에서 상황에 맞는 페이지 교체를 진행하기 위해 페이지 교체 알고리즘이 존재함

## 페이지 교체 알고리즘의 종류
- OPT - Optimal: 앞으로 가장 오랫동안 사용되지 않을 페이지 교체
- FIFO - First In First Out
- LRU - Least Recently User: 가장 오랫동안 사용되지 않은 페이지 교체
- LFU - Least Frequently Used: 참조 횟수가 가장 작은 페이지 교채
- MFU - Most Frequently Used: 참조 횟수가 가장 많은 페이지 교체
- NUR - Not Used Recently: 최근에 사용하지 않은 페이지 교체

### OPT
- 가장 이상적
- FIFO에 비해 페이지 결함의 횟수를 많이 감소시킬 수 있음
- 프로세스가 앞으로 사용할 페이지를 미리 알아야 함 -> 불가능
- 비교 연구 목적을 위해 사용

<img width="759" height="296" alt="image" src="https://github.com/user-attachments/assets/89b4be0d-6807-4e57-9dd9-7c1cf891a9bf" />

### FIFO
- victim page: out되는 페이지는 가장 먼저 메모리에 올라온 페이지
- 가장 간단한 방법으로, 특히 초기화 코두에서 적절한 방법 
- `초기화 코드`: 처음 프로세스 실행될 때 최초 초기화를 시키는 역할만 진행하고 다른 역할은 수행하지 않으므로, 메인 메모리에서 빼도 괜찮음
- 하지만 처음 프로세스 실행시에는 무조건 필요한 코드이므로, FiFO 알고리즘을 사용하면 초기화를 시켜준 후 가장 먼저 내보내는 것이 가능

<img width="763" height="297" alt="image" src="https://github.com/user-attachments/assets/b800c990-1888-4ede-bdf5-71d6799c21c5" />

### LRU
- 최근에 사용하지 않았으면, 나중에도 사용되지 않을 것이라는 아이디어에서 나옴
- OPT의 경우 미래 예측이지만, LRU의 경우 과거를 보고 판단하므로 실질적으로 사용이 가능한 알고리즘
  - (실제로 최근에 사용하지 않은 페이지는 앞으로도 사용하지 않을 확률이 높다)
- OPT보다는 페이지 결함이 더 일어날 수 있지만, **실제로 사용할 수 있는 페이지 교체 알고리즘 중에서는 가장 좋은 방법 중 하나**
  
<img width="780" height="304" alt="image" src="https://github.com/user-attachments/assets/1796bcaf-7913-4908-b997-8bc756c95062" />

### LFU
- 페이지의 참조 횟수로 교체할 페이지 결정
- LRU는 직전 참조된 시점만을 반영하지만, LFU는 참조 횟수를 통해 장기적 시간규모에서의 참조 성향을 고려할 수 있음
- 가장 최근에 불러온 페이지가 교체될 수 있고, 구현이 복잡하며 오버헤드가 크다는 단점이 있다

### MFU
- 가장 많이 사용된 페이지가 앞으로는 사용되지 않는다는 가정을 가지고 교체할 페이지를 결정

### NUR = NRU(Not Used Recently, Not Recently Used), 클럭 알고리즘
<img width="800" height="376" alt="image" src="https://github.com/user-attachments/assets/25eb72cf-8cdc-4785-8f67-551f6ff77830" />

- 최근에 사용하지 않은 페이지 교체 (LRU를 근사한 알고리즘)
- 교체되는 페이지의 참조 시점이 가장 오래되었다는 것을 보장하지 못한다
- 적은 오버헤드로 적절한 성능
- 동일 그룹 내 무작위 선택
- 각 페이지별로 두개의 비트가 사용됨
  - 참조 비트 (Reference Bit)
    - 페이지가 참조되지 않았을 때 0, 호출 되었을 때 1 (모든 참조 비트를 주기적으로 0으로 변경)
  - 변형 비트 (Modified Bit, Birty Bit)
    - 페이지 내용이 변경되지 않았을 때 0, 변경되었을 때 1
  - 우선순위 : 참조비트 > 변형비트

## 교체 방식
- Global 교체
  - 메모리 상의 모든 프로세스 페이지에 대해 교체하는 방식
- Local 교체
  - 메모리 상의 자기 프로세스 페이지에서만 교체하는 방식

다중 프로그래밍의 경우, 메인 메모리에 다양한 프로세스가 동시에 올라올 수 있음. 따라서 다양한 프로세스의 페이지가 메모리에 존재

페이지 교체 시, 다양한 페이지 교체 알고리즘을 활용해 victim page를 선정하는데, 선정 기준을 글로벌로 하느냐 로컬로 하느냐에 대한 차이

-> 실제로는 전체를 기준으로 페이지를 교체하는 것이 더 효율적이라고 함. 자기 프로세스 페이지에서만 교체 하면 교체를 해야할 때 각각 모두 교체를 진행해야 하므로 비효율적이라고 한다.


## Ref
- [기출로 대비하는 개발자 전공면접](https://www.inflearn.com/course/%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%A0%84%EA%B3%B5%EB%A9%B4%EC%A0%91-cs-%EC%99%84%EC%A0%84%EC%A0%95%EB%B3%B5/dashboard)
- [tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/Process%20Management%20%26%20PCB.md)
- https://doh-an.tistory.com/28#google_vignette
