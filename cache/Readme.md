# Cache

## Other Content
- [캐시 문제 해결 가이드 - DB 과부하 방지 실전 팁](https://github.com/dh1010a/study/blob/main/cache/cache-problem.md)

## 목차
1. 캐시란?
2. 읽기 전략
3. 쓰기 전략
4. 읽기 + 쓰기 전략 조합

# 1. 캐시란?
캐시(Cache) : Temporary Location For Speed

데이터의 원래 소스보다 더 빠르고 효율적으로 엑세스할 수 있는 임시 데이터 저장소

- 원본(Origin)보다 빠른 접근 속도
- 같은 데이터에 대한 반복적인 엑세스
- 잘 변하지 않는 데이터

Redis는 캐시로 활용하기에 가장 효과적이고 효율적인 소프트웨어

1. 빠른 성능
2. 평균 작업속도 < 1ms
3. 초당 수백만건의 작업 가능

# 2. 읽기 전략
## Look-Aside(Lazy Loading)
* 애플리케이션에 데이터를 읽는 작업이 많을 때 활용되는 전략이며, Redis를 캐시로 사용할 때 가장 많이 쓰는 전략
![image](https://github.com/user-attachments/assets/a9479d27-2673-4c50-8136-7bb58f129a34)

* 애플리케이션은 데이터를 찾을 때 redis를 먼저 확인하고 있는 경우 해당 데이터를 redis에서 가져옴
![image](https://github.com/user-attachments/assets/0aabf618-44b7-4f14-9723-52558e5d05e6)

- 만약 redis에 찾는 키가 없다면 DB에서 조회해서 값을 가져온 뒤, 다시 redis에 저장
- 따라서 찾는 데이터가 없을 때에만 redis에 값을 저장하기 때문에 Lazy Loading이라고도 부름
- 이 구조는 redis가 다운되더라도 바로 장애가 일어나지 않고 DB에서 값을 조회할 수 있음
- cache로 붙어있던 connection이 많았다면 이 connection이 모두 DB로 연결되기 때문에 DB에 갑자기 많은 부하가 발생할 수 있음
- 따라서 캐시를 새로 투입하거나 DB에 새로운 값을 저장한 경우 Cache Miss가 증가하기 때문에 성능 저하를 유발할 수 있음
![image](https://github.com/user-attachments/assets/e48c6b21-e650-45b8-b019-bd5a4234d824)

 - 이럴 때에는 미리 DB에서 cache로 데이터를 밀어 넣어주는 Cache Warming 작업을 진행

> **Cache Warming**
>
> 미리 cache로 db의 데이터를 밀어 넣어두는 작업을 의미한다.이 작업을 수행하지 않으면 서비스 초기에 트래픽 급증시 대량의 cache miss 가 발생하여 데이터베이스 부하가 급증 할 수 있다. (Thundering Herd)다만, 캐시 자체는 용량이 작아 무한정으로 데이터를 들고 있을수는 없어 일정시간이 지나면 expire되는데, 그러면 다시 Thundering Herd가 발생될 수 있기 때문에 캐시의 TTL을 잘 조정할 필요가 있다.

## Read Through
- 캐시에서만 데이터를 읽어오는 전략 (inline cache)
- Look Aside 와 비슷하지만 데이터 동기화를 라이브러리 또는 캐시 제공자에게 위임하는 방식이라는 차이가 있음.
- 따라서 데이터를 조회하는데 있어 전체적으로 속도가 느림.
- 또한 데이터 조회를 전적으로 캐시에만 의지하므로, redis가 다운될 경우 서비스 이용에 차질이 생길수 있음.
- 대신에 캐시와 DB간의 데이터 동기화가 항상 이루어져 데이터 정합성 문제에서 벗어날수 있음
- 읽기가 많은 워크로드에 적합

![image](https://github.com/user-attachments/assets/3264ae6d-4daa-42df-b886-4e2aa21f440e)

- Read Through 방식은 Cache Aside 방식과 비슷하지만, Cache Store에 저장하는 주체가 Server이냐 또는 Data Store 자체이냐에서 차이가 있음
- 이 방식은 직접적인 데이터베이스 접근을 최소화하고 Read 에 대한 소모되는 자원을 최소화할 수 있음
- 하지만 캐시에 문제가 발생하였을 경우 이는 바로 서비스 전체 중단으로 빠질 수 있음. 그렇기 때문에 redis과 같은 구성 요소를 Replication 또는 Cluster로 구성하여 가용성을 높여야 함 

# 3. 쓰기 전략
## Write Back
- Write Behind 패턴 이라고도 불림.
- 캐시와 DB 동기화를 비동기하기 때문에 동기화 과정이 생략
- 데이터를 저장할때 DB에 바로 쿼리하지않고, 캐시에 모아서 일정 주기 배치 작업을 통해 DB에 반영
- 캐시에 모아놨다가 DB에 쓰기 때문에 쓰기 쿼리 회수 비용과 부하를 줄일 수 있음
- Write가 빈번하면서 Read를 하는데 많은 양의 Resource가 소모되는 서비스에 적합
- 데이터 정합성 확보
- 자주 사용되지 않는 불필요한 리소스 저장.
- 캐시에서 오류가 발생하면 데이터를 영구 소실함.

  ![image](https://github.com/user-attachments/assets/ce8ac6fb-4dfa-4b2a-a394-50a2d72b865a)

Write Back 방식은 데이터를 저장할때 DB가 아닌 먼저 캐시에 저장하여 모아놓았다가 특정 시점마다 DB로 쓰는 방식으로 캐시가 일종의 Queue 역할을 겸하게 된다.

캐시에 데이터를 모았다가 한 번에 DB에 저장하기 때문에 DB 쓰기 횟수 비용과 부하를 줄일 수 있지만, 데이터를 옮기기 전에 캐시 장애가 발생하면 데이터 유실이 발생할 수 있다는 단점이 존재한다. 하지만 오히려 반대로 데이터베이스에 장애가 발생하더라도 지속적인 서비스를 제공할 수 있도록 보장하기도 한다.

이 전략 또한 Cache 서비스의 가용성을 높이는 것이 좋으며, 캐시 읽기 전략인 Read-Through와 결합하면 가장 최근에 업데이트된 데이터를 항상 캐시에서 사용할 수 있는 혼합 워크로드에 적합하다.

## Write Through 전략
- 데이터베이스와 Cache에 동시에 데이터를 저장하는 전략
- 데이터를 저장할 때 먼저 캐시에 저장한 다음 바로 DB에 저장 (모아놓았다가 나중에 저장이 아닌 바로 저장)
- Read Through 와 마찬가지로 DB 동기화 작업을 캐시에게 위임
- DB와 캐시가 항상 동기화 되어 있어, 캐시의 데이터는 항상 최신 상태로 유지
- 캐시와 백업 저장소에 업데이트를 같이 하여 데이터 일관성을 유지할 수 있어서 안정적
- 데이터 유실이 발생하면 안 되는 상황에 적합
- 자주 사용되지 않는 불필요한 리소스 저장.
- 매 요청마다 두번의 Write가 발생하게 됨으로써 빈번한 생성, 수정이 발생하는 서비스에서는 성능 이슈 발생
- 기억장치 속도가 느릴 경우, 데이터를 기록할 때 CPU가 대기하는 시간이 필요하기 때문에 성능 감소

Write Through 패턴은 Cache Store에도 반영하고 Data Store에도 동시에 반영하는 방식이다. (Write Back은 일정 시간을 두고 나중에 한꺼번에 저장)
그래서 항상 동기화가 되어 있어 항상 최신정보를 가지고 있다는 장점이 있다.
하지만 결국 저장할때마다 2단계 과정을 거쳐치기 때문에 상대적으로 느리며, 무조건 일단 Cache Store에 저장하기 때문에 캐시에 넣은 데이터를 저장만 하고 사용하지 않을 가능성이 있어서 리소스 낭비 가능성이 있다.

> write throuth 패턴과 write back 패턴 둘 다 모두 자주 사용되지 않는 데이터가 저장되어 리소스 낭비가 발생되는 문제점을 안고 있기 때문에, 이를 해결하기 위해 TTL을 꼭 사용하여 사용되지 않는 데이터를 반드시 삭제해야 한다. (expire 명령어)

## Write Around
- Write Through 보다 훨씬 빠름
- 모든 데이터는 DB에 저장 (캐시를 갱신하지 않음)
- Cache miss가 발생하는 경우에만 DB와 캐시에도 데이터를 저장
- 따라서 캐시와 DB 내의 데이터가 다를 수 있음 (데이터 불일치)

Write Around 패턴은 속도가 빠르지만, cache miss가 발생하기 전에 데이터베이스에 저장된 데이터가 수정되었을 때, 사용자가 조회하는 cache와 데이터베이스 간의 데이터 불일치가 발생하게 된다. 따라서 데이터베이스에 저장된 데이터가 수정, 삭제될 때마다, Cache 또한 삭제하거나 변경해야 하며, Cache의 expire를 짧게 조정하는 식으로 대처해야 한다.

Write Around 패턴은 주로 Look aside, Read through와 결합해서 사용된다.데이터가 한 번 쓰여지고, 덜 자주 읽히거나 읽지 않는 상황에서 좋은 성능을 제공

# 4. 읽기 + 쓰기 전략 조합
1. Look Aside + Write Around
  - 일반적으로 자주 사용되는 전략.
2. Read Through + Write Around
  - 항상 DB에 쓰고, 캐시에서 읽을때 항상 DB에서 먼저 읽어오므로 데이터 정합성 이슈에 대한 안전 장치를 구성할 수 있음
3. Read Through + Write Through 조합
  - 데이터를 쓸때 항상 캐시에 먼저 쓰므로, 읽어올때 최신 캐시 데이터 보장
  - 데이터를 쓸때 항상 캐시에서 DB로 보내므로, 데이터 정합성 보장
