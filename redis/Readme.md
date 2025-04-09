# Other Content
1. [레디스 스트림](https://github.com/dh1010a/study/blob/main/redis/redis-stream.md)

<br><br>

# Redis 알아보기

## 목차
1. 레디스란?
2. 레디스의 데이터 타입
3. 레디스 데이터 백업 전략
4. 레디스 아키텍처
5. 레디스 운영 및 장애 포인트


# 1. Redis란?
- 고성능 Key-Value 구조의 저장소
- 비정형 데이터를 저장, 관리하기 위한 오픈 소스 기반의 NoSQL
- In-Memory 데이터 구조를 가진 저장소
- DB, Cache, Message Queue, Shared Memory 용도로 사용

> **인메모리 ( In-Memory )**
> - 컴퓨터의 주기억장치인 RAM에 데이터를 올려서 사용하는 방법
> - RAM에 데이터를 저장하게 되면 메모리 내부에서 처리가 되므로데이터를 저장/조회할 때 하드디스크를 오고가는 과정을 거치지 않아도 되어 **속도가 빠름**
> - 그러나 서버의 메모리 용량을 초과하는 데이터를 처리할 경우,RAM의 특성인 **휘발성**에 따라 **데이터가 유실**될 수 있음

레디스는 인메모리 데이터 저장소가 가지는 휘발성의 특성으로 데이터가 유실될 경우를 방지하여 백업 기능을 제공

### 싱글 스레드

레디스는 싱글 스레드로 모든 명령을 처리함. 한번에 하나의 명령만 수행이 가능하므로 Race Condition이 거의 발생하지 않음

> **Race condition**
>
> - 공유 자원에 대해 여러 프로세스가 동시에 접근을 시도할 때, 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태
> - 즉, 두개의 스레드가 하나의 자원을 놓고 서로 사용하려고 경쟁하는 상황에서 발생
> - 프로그램의 일관성과 정확성을 손상시킬 수 있음 
> - Ex:  두개의 스레드 A, B가 X라는 공유변수를 사용하는데, A의 역할은 X에 `+ 1` 후 저장 / B의 역할은 X에 `1` 후 저장일 때 A가 X를 읽은 후 B가 실행되기 전에 결과를 저장한다고 하면, A의 결과가 스레드 B의 결과데 덮어씌워지게 됨
> 
> [참고 자료](https://iredays.tistory.com/125)

Redis는 싱글 스레드를 사용해 Race Condition이 거의 발생하지 않는다는 장점이 있다. 하지만, 싱글 스레드는 멀티 스레드에 비해 속도의 한계가 명확하다.
이 문제를 해결 하기 위해 Redis 6.0에서는 **ThreadedIO**가 도입되어, 기존보다 속도가 약 2.5배 빨라졌다고 한다. 메모리 내부에서 명령의 실행 자체는 싱글 스레드로 동작하나,

- 클라이언트가 전송한 명령을 네트워크로 읽어서 파싱하는 부분
- 명령이 처리된 결과 메시지를 클라이언트에게 네트워크로 전달하는 부분

에 ThreadedIO가 적용

자세한 내용은 아래 참고

- [참고자료](https://charsyam.wordpress.com/2020/05/05/%EC%9E%85-%EA%B0%9C%EB%B0%9C-redis-6-0-threadedio%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90/)

## 사용시 주의할 점
- **시간 복잡도**
  - Single Threaded(싱글 스레드) 사용으로 한번에 하나의 명령만 수행이 가능하므로 처리 시간이 긴 요청의 경우 장애가 발생
    
- **메모리 단편화**
  - 크고 작은 데이터를 할당하고 해제하는 과정에서 메모리의 파편화가 발생하여 응답 속도가 느려질 수 있음

- **주기적인 모니터링**
  - 메모리 사용량이 너무 많으면 Redis 서버의 성능 저하나 장애로 이어질 수 있기 때문에 주기적인 모니터링을 통해 메모리를 관리해주어야함 

> **메모리 단편화**
> ![image](https://github.com/user-attachments/assets/a871623e-e345-49cb-a4cd-12fb684bad66)
> - RAM에서 메모리를 할당받고 해제하는 과정에서 위와같이 부분부분 빈 공간이 생기는데,
새로운 메모리 할당 시에 사용 가능한 메모리가 충분히 존재하지만 메모리의 크기만큼의 부분이 없어 마지막 부분에 할당되어 메모리 낭비가 심하게 됨
> - 이 현상이 계속되면 실제 physical 메모리가 커져 프로세스가 죽는 현상이 발생 할 수도 있으므로, redis를 사용 시에 메모리를 적당히 여유있게 사용하는 것이 좋음

# 2. 레디스 데이터 타입
Redis는 자체적으로 다양한 자료구조를 제공한다.
![image](https://github.com/user-attachments/assets/fca79298-d873-4ce9-baab-db489ca37402)

|자료형|특징|
|---|---|
|Strings|- 가장 기본적인 형태<br>- Set command이용해 저장되는 데이터는 모두 string 형태|
|Bitmaps|- String의 변형<br>- bit 단위 연산 가능|
|Lists|- 데이터를 순서대로 저장<br>- queue로 사용하기 적절|
|Hashes|- 하나의 key안에 다시 key-value 형태 저장|
|Sets|- 중복되지 않는 문자열의 집합|
|Sorted Sets|- Sets과 기본적으로 동일<br>- 모든 값은 score라는 숫자 값으로 정렬<br>- 데이터가 저장될 때부터 score 순으로 정렬<br>- score가 같을 때에는 사전순으로 정렬|
|HyperLogLogs|- 굉장히 많은양의 데이터를 다룰 때 사용<br>- 중복되지 않는 데이터를 count할때 주로 많이 사용|
|Streams|- 로그를 저장하기 가장 좋은 자료 구조|

## 실 사용 예시 - Counting
#### Strings
- 단순 증감 연산
- `INCR`, `INCRBY`
#### Bits
- 데이터 저장공간 절약
- 예를 들어 오늘 접속한 사용자의 수를 저장할 때 날짜 키 하나를 만들고 userId의 비트를 증가
- 사용자가 1000만명일 때 1000만 bit = 1.2MB
- `SETBIT`, `BITCOUNT`
#### HyperLogLogs
- 대용량 데이터를 카운팅할 때 적절
- set과 비슷하지만 저장되는 용량은 매우 작음(12KB 고정)
- 저장된 데이터는 다시 확인할 수 없음
- `PFADD`, `PFCOUNT`, `PFMERGE`

## 실 사용 예시 - Message
#### Lists
- Message queue로 활용하기에 적절
- 자체적으로 Blocking 기능을 제공하기 때문에 불필요한 polling process를 방지할 수 있음
- `BRPOP`, `LPUSH`
- 키가 있을 때만 저장 가능 - `LPUSHX` / `RPUSHX`
- 이미 캐싱되어 있는 피드에만 신규 트윗을 저장

#### Streams
- 로그를 저장하기 가장 적절한 자료구조
- append-only
- 시간 범위로 검색 / 신규 추가 데이터 수신 / 소비자별 다른 데이터 수신(소비자 그룹)
- `XADD`

# 3. 레디스 데이터 백업 전략
redis에서는 데이터를 영구 저장하기 위해 두 가지 옵션 제공

## AOF(Append Only File)
- 데이터를 변경하는 명령어가 들어오면 그것을 그대로 파일로 관리
- 디스크에 appendonly.aof파일로 남김
- non-blocking으로 동작하며, append-only이므로 write 속도가 빠르다
- AOF 파일은 계속해서 추가되기만 하고 텍스트 형식이기 때문에 대부분 RDB파일보다 커지게 됨
- 따라서 AOF 파일은 주기적으로 압축해서 재작성해야 됨
- 실제 저장은 Redis protocol 형태로 저장
- text파일이라 무겁기 때문에, `redis.conf`에 용량을 제한하거나 rewrite 기능 등을 설정해야 함


### 저장 방법
- 자동 : redis.conf 파일에서 `auto-aof-rewrite-percentege` 옵션(크기 기준)
- 수동 : `BGREWRITEAOF` 커맨드를 이용해 CLI 창에서 수동으로 AOF 파일 재작성

### appendfsync
AOF는 버퍼 캐시에 저장하고 적절한 시점에 이 데이터를 디스크로 저장하는데, appendfsync로 디스크와 동기화를 얼마나 자주 할 것인지 설정할 수 있다.
- **always**: 명령 실행 시 마다 AOF 기록. 정합성은 높지만 성능이 매우 떨어진다.
- **everysec**: 1초마다 AOF에 기록. 권장
- **no**: AOF 기록 시점을 OS가 정한다.(일반적인 리눅스 디스크 기록 간격은 30초) 데이터가 유실될 수 있다.

### rewrite
AOF 파일의 상태가 특정 조건일 때 AOF 파일을 현재 상태에 맞춰, 설정에 따라 덮어쓰기 하거나 새로 생성하도록 한다.
  
## RDB(snapshat)
- 명령어를 저장하지 않고 저장 당시에 메모리를 그대로 파일로 관리
- 실제 저장은 binary 형태로 저장

### 저장 방법
자동 : redis.conf 파일에서 SAVE 옵션(시간 기준)
수동 : `BGSAVE` 커맨드를 이용해서 CLI 창에서 수동으로 RDB 파일 저장, `SAVE` 커맨드는 절대 사용 X

### 스냅샷 방식 두가지
- `SAVE`: blocking으로 redis 동작을 정지시키고, snapshot을 저장
  1. Main process가 데이터를 새 RDB temp 파일에 쓴다.
  2. 쓰기가 끝나면 기존 파일을 지우고 새 파일로 교체한다.

- `BGSAVE`: non-blocking으로, 별도의 자식 프로세스(백그라운드 SAVE)를 띄운 후, 명령어 수행 당시의 snapshot을 저장
- redis는 동작을 멈추지 않는다. `fork()`를 하기 때문에 메모리를 두 배정도 사용하므로 주의해야함
  1. Child process를 fork()한다.
  2. Child process는 데이터를 새 RDB temp 파일에 쓴다.
  3. 쓰기가 끝나면 기존 파일을 지우고, 이름을 변경한다.

## 선택 기준
- 레디스를 단순 캐시로만 사용하는 경우 백업은 불필요함. 
- **백업은 필요하지만 어느 정도의 데이터 손실이 발생해도 괜찮은 경우**라면 RDB 단독 사용하고, redis.conf 파일에서 SAVE 옵션을 적절히 사용
  - ex. SAVE 900 1
- **장애 상황 직전까지의 모든 데이터가 보장되어야 할 경우**라면 AOF 사용. `APPENDSYNC` 옵션이 `everysec`인 경우 최대 1초 사이의 데이터 유실 가능(기본 설정)
- **제일 강력한 내구성이 필요한 경우,** RDB와 AOF 모두 적절히 사용

# 4. 레디스 아키텍처
![image](https://github.com/user-attachments/assets/3d4e0445-03b5-4e61-98c1-6017140a2d4f)

## Replication
- 단순한 복제 연결
- replicaof 커맨드를 이용해 간단하게 복제 연결
- 비동기식 복제
- HA 기능이 없으므로 장애 상황 시 수동 복구
  - replicaof no one
  - 애플리케이션에서 연결 정보 변경

> HA(High Availablity)? 고가용성, 백업 또는 장애극복 처리를 의미

## Sentinel
- Redis 센티넬은 고가용성에 초점을 두고 있고, 레디스 클러스터는 확장성에 초점을 두고 있음
- 센티넬은 RDBMS에서 높은 가용성을 위해 사용하는 전략인 Replication을 도와주기 위한 설정 같은것
- 마스터, 슬레이브, 센티넬로 구성이 되어있다. 센티넬은 마스터와 슬레이브를 모니터링 해주고, 레디스 펍섭이나 쉘 스크립트를 사용하여 알림도 보내줄 수 있다
- 센티넬은 3개 이상의 노드로 구성하는 것이 좋다. 왜냐하면 그들이 Failover를 해야할 주체나 장애 감지를 해야할 때 과반수 투표를 기반으로 의사결정을 하는 의회 방식을 사용하기 때문에

### 동작 과정
1. Sentinel이 마스터 Redis 감시
2. SDown 판단 (개별 Sentinel)
3. ODown 판단 (quorum 다수 동의)
4. Sentinel 중 하나가 리더로 선출
5. 슬레이브 중 하나를 승격해 새 마스터로 지정
6. 다른 슬레이브는 새로운 마스터로 재동기화
7. 클라이언트에 새로운 마스터 정보를 전파

### Failover 과정
- 센티넬의 동작 방식은, 센티넬들이 마스터에 Ping을 보내고 설정해준 시간`down-after-milliseconds` 동안 응답이 없을 경우 장애 상태로 판단하여 `SDOWN`을 기록한다. 
- quorum 이상의 노드가 SDOWN 상태라고 판단이 되었을때 객관적으로 얘는 죽었다 라고 판단하는 `ODOWN` 상태가 된다.

이때 투표를 통해 Leader Sentinel을 선출한다. 모든 센티넬은 본인이 리더가 되길 원하며 `is-master-down-by-add`를 통해 본인의 `runid`를 보내고, `failover-timeout` 안에 승자를 결정하며, 가장 먼저 최다 득표를 받은 센티넬이 리더가 된다.

리더는 하나의 살아있는 슬레이브를 골라 페일오버를 시작한다.
- 응답 가능(slave가 살아 있음)
- replication offset이 가장 큰 것 (데이터 최신)

리더는 나머지 슬레이브에게`SLAVEOF NO ONE`으로 새 마스터를 알리고 `SLAVEOF <new-master>` 명령을 보내게 되며, `parallel-syncs` 수 만큼 병렬로 동기화를 시작한다.
- 살아있고, 최근 동기화 상태가 최신이고, `priority`가 낮은 것
- 센티넬은 `get-master-addr-by-name`을 클라이언트에 보내 새로운 마스터를 알린다.
- 모든 Sentinel과 클라이언트는 `SENTINEL get-master-addr-by-name` 로 마스터 IP를 가져옴

```
     [Sentinel A]       [Sentinel B]       [Sentinel C]
       ↓                ↓                ↓
 [PING master]    [PING master]    [PING master]
       ↓                ↓                ↓
   SDown A          SDown B          SDown C
       ↓                ↓                ↓
     Vote            Vote              Vote
       ↘︎              ↓               ↙
         → B gets majority vote → becomes Leader

            [Leader Sentinel: B]
                   |
          Promote slave X to master
                   |
     SLAVEOF NO ONE on X, others → SLAVEOF X
                   |
  Inform all sentinels + update config epoch
```

### Failover가 실패하는 경우
| 원인 | 설명 |
| --- | --- |
| quorum 수 미달 | 일부 Sentinel만 장애를 인식한 경우 |
| 승격할 슬레이브 없음 | 슬레이브들도 모두 다운된 경우 |
| 동기화 실패 | 슬레이브가 새 마스터와 sync 실패 |
| 리더 Sentinel 선정 실패 | 투표가 무효/충돌 |

- 이런 상황에서 페일오버가 실패할 수 있다.
- `failover-timeout` 시간 내에 안 끝나면 실패 처리되고, 다시 시도함

- 또한 슬레이브가 없는 상황을 방지하기 위해 아래와 같은 설정이 가능함

| 설정명 | 의미 |
| --- | --- |
| `min-replicas-to-write` | 쓰기를 허용하려면 최소한 이 숫자만큼의 슬레이브가 붙어있어야 함 |
| `min-replicas-max-lag` | 각 슬레이브는 마스터와 이 시간(ms) 이하로만 지연되어야 함 |

마스터가 이 설정을 보고, 슬레이브가 충분하지 않으면 쓰기를 아예 막아버려 데이터의 유실을 방지한다.


### 레디스의 복제
이때 Replication에 복제는 어떻게 하고있는가?
- Redis는 기본적으로 **비동기 복제**(asynchronous replication)를 하고있음
- 마스터에서 데이터를 변경하면,replication buffer하고  **복제 로그(replication stream)**가 슬레이브로 전송
- 슬레이브는 그것을 받아 마스터와 동일하게 상태를 유지
- `SLAVEOF`라는 명령어로 처음 붙으면, 데이터가 없거나 오래된 경우 마스터가 **RDB 스냅샷을 생성하여 전송**하고, 슬레이브는 그걸 받아서 초기화

### Partial Sync
- 마스터는 각 슬레이브에 replication offset을 추적하여 슬레이브가 잠깐 끊겼다면 그 지점부터 다시 전송
- Redis Replication Stream은 마스터 Redis에서 발생한 **모든 쓰기 명령(CUD)**을 실시간으로 **슬레이브에게 전송**하는 스트림
- 이건 단순히 데이터를 "복사"하는 게 아니라, **마스터가 수행한 명령 자체를 슬레이브에게 그대로 전송해서, 슬레이브가 같은 상태를 재현**하는 방식
- 슬레이브는 마스터에 TCP 연결을 맺고, PSYNC로 통신. 만약 처음이면 `PSYNC ? -1`로 전체, 연결된 적있다면 `PSYNC <replicationId> <offset>`로 차이만 요청.
- 마스터는 명령어가 작동될 때 마다 **command stream**으로 전달. 이 stream은 일반 텍스트 기반 Redis protocol (RESP)을 사용

### 리더가 동시에 여러개가 선출될 수 있는 경우
- **원칙적으로 안 됨.** 하지만 예외 상황에서는 가능성 있음
  - 네트워크 파티션(Network Split) 발생
  - config epoch가 꼬인 상태에서 동시 투표 발생
- Sentinel은 **config epoch와 투표 기록**을 통해 중복 failover 방지함

### 센티넬의 감시 대상
- 센티넬은 자신이 `monitor`로 등록한 마스터와 그 마스터에 연결된 슬레이브들까지 감시함
- 직접적으로 등록한 마스터를 감시하다가, 마스터 Redis는 `INFO replication` 명령을 통해 자신의 슬레이브 목록을 알려줄 수 있음. 그래서 자동으로 수집해서 이것도 같이 간접적으로 감시
- 슬레이브에게도 아래와 같은것을 PING 보냄

| 체크 항목 | 설명 |
| --- | --- |
| 슬레이브 상태 | 살아있는지, 최신 데이터를 유지하는지 |
| replication 상태 | 마스터와 얼마나 sync되어 있는지 |
| failover 대상 적합성 | 새로운 마스터로 승격 가능한지 판단할 때 기준으로 사용 |

### 센티넬을 도커로 띄울 때 주의사항
근데 센티넬이 도커 내부에 있다면, 매핑 주소가 다름. 실제 센티넬은 2345 포트여도 외부에선 3305를 볼 수도 있음. 그럼 얘는 다른 센티넬에 실제 자기 포트인 2345를 알려주기 때문에 다른 센티넬은 그것을 호출했다가 정상적으로 통신할 수 없음

```
sentinel announce-ip 192.168.1.10
sentinel announce-port 26380
```

- 그래서 이것을 설정해줘서 본인이 어디에 있다는걸 정확히 알려줄 수 있음. 이것을 통해 NAT 게이트웨이 환경에서도 적절히 동작할 수 있게됨.
- 그리고 센티넬은 센티넬 모드로 실행할 수 있고, env를 주입해서 필요한 설정을 함.

## Cluster
- 여러 Redis 노드를 묶어서 하나처럼 사용하는 구조
- 데이터 분산(Sharding) + 고가용성(Failover)까지 지원
- 모든 노드가 서로를 감시하여 마스터가 비정상 상태일 때 자동으로 failover
- 최소 3대의 마스터 노드가 필요
- Cluster는 하나의 마스터노드와 최소 하나의 슬레이브노드가 쌍을 이뤄 클러스터를 이루고 이런 클러스터가 여러개 있는 형태

### Hash Slot
Hash Slot은 총 16384개를 가질 수 있고 이론적으로는 이 슬롯 하나에 하나의 노드를 가질 수 있어 총 1만6천개가량의 노드을 이론적으로 가질 수 있지만, 공식 문서상 1000개를 넘지 않는게 좋다. `CRC16`이라는 해싱 함수를 사용

1. `user:1000`라는 키가 조회된다.
2. Cluster는 해당 키가 어떤 슬롯에 있는지 검사한다.
3. 1567번 슬롯을 관리하는 클러스터를 호출한다.
4. 해당 클러스터에서 값을 가져온다.

### Hash Tag - 해시 슬롯과 따라다니는 개념
- Redis의 Hash Tag는 어떤 값을 해시슬롯에 넣을지 결정하는 일종의 라벨. 이 태그가 붙어있는 것은 해시슬롯에 들어가며 Redis Cluster의 관리를 받는다
- Redis에서는 키에 중괄호가 들어가는 상황을 대비해 여러가지 규칙이 있음. 예를 들어서 {{user:1000}}이렇게 키를 설정하면 첫번째 열린 중괄호를 기준으로 두번째 열린 중괄호는 무시하고 첫번째로 나온 닫는 중괄호까지 해시슬롯에 저장

이런 일련의 과정을 자동으로 처리해주고 이 과정을 오토샤딩이라고 함. 이런 강력한 능력 덕분에 높은 확장성을 보여줄 수 있음. 그냥 클러스터를 옆으로 늘리기만 하면 데이터도 알아서 해시슬롯에 보관되고 성능까지 올라감

### 새로운 클러스터를 추가/삭제 하는 경우
- 그럼 노드를 추가하거나 지울때 해시 슬롯에 변화를 주려면, 기존에 있던건 어떻게?
- 슬롯을 새로 만들면, 해시 슬롯이 **자동으로 재분배되지 않음, 직접 수동으로 해시 슬롯을 옮겨줘야 함**
- `redis-cli --cluster reshard`

- Redis는 이를 방지하기 위해 반드시**슬롯 이동 시 MIGRATE 명령을 자동으로 수행** (직접 RESTORE/DUMP 하거나 클러스터 명령으로 처리 가능)

### 리다이렉트

| 리다이렉션 타입 | 언제 발생? | 설명 |
| --- | --- | --- |
| `-MOVED` | 슬롯 소유 노드가 **영구적으로 바뀐 후** | 완전히 옮겨졌음! 다음부턴 이 노드로 바로 가라 |
| `-ASK` | 슬롯 이동(MIGRATE) **도중** | 일시적 상태. **한 번만** 다른 노드로 가서 처리해줘 |

-ask가 유용할 떄

| 케이스 | 효과 |
| --- | --- |
| 슬롯을 수만 개씩 점진적으로 재배치할 때 | 실시간 서비스 끊김 없이 슬롯 이동 가능 |
| 티켓팅, 예약 등 높은 실시간성이 필요할 때 | 순간 리다이렉션으로 부드럽게 대응 |

## 선택 기준
![image](https://github.com/user-attachments/assets/b2a9f5be-41f2-42e8-af7c-297cbbe11090)

#### 자동 failover 필요 & Scale Out 필요(샤딩)
- Cluster

#### 자동 failover 필요 & Scale Out 필요 X(샤딩)
- Sentinel
  
#### 자동 failover 필요 X & 복제 필요
- Replication

#### 자동 failover 필요 X & 복제 필요 X
- Stand-Alone

# 5. Redis 운영 Tip 및 장애 포인트
## 사용하면 안 되는 커맨드
### Redis는 Single Thread로 동작
- `keys *` → `scan`으로 대체
- Hash나 Sorted Set 등 자료구조
    - 키 내부에 아이템이 많아질수록 성능이 저하
    - 성능을 고려한다면 키를 나누어서 하나의 키에 최대 100만개까지만 아이템을 저장
    - `hgetall` → `hscan`
    - 키에 많은 아이템이 저장되어 있을 때 `del`로 지우면 해당 키를 지우는동안 아무런 동작을 할 수가 없음
        - 이 때 `unlink` 커맨드를 사용하면 백그라운드에서 키를 삭제

## 변경하면 장애를 막을 수 있는 기본 설정값
### `STOP-WRITES-ON-BGSAVE-ERROR = NO`
- Default 설정은 yes
- RDB 파일 저장 실패 시 redis로 들어오는 모든 write를 차단하는 옵션

### `MAXMEMORY-POLICY = ALLKEYS-LRU`
- redis를 캐시로 사용할 때 expire time 설정 권장
- 메모리가 가득 찼을 때 MAXMEMORY-POLICY 정책에 의해 키 관리
    - `NOEVICTION`(default) : 삭제 안함 → 더 이상 redis에 새로운 키를 저장하지 않는 것을 의미 
    - `VOLATILE-LRU` : LRU 정책, 가장 최근에 사용하지 않았던 key부터 삭제, expire 설정에 있는 key만 삭제
    - `ALLKEYS-LRU` : 모든 key에 대해 expire에 상관없이 LRU 정책 적용

## MaxMemory 값 설정
### Persistence / 복제 사용 시 MaxMemory 설정 주의
- RDB 저장 & AOF rewrite 시 `fork()`
- Copy-on-Write로 인해 메모리를 두 배로 사용하는 경우 발생 가능
- Persistence / 복제 사용 시 MaxMemory는 실제 메모리의 절반으로 설정

## Memory 관리
### 물리적으로 사용되고 있는 메모리를 모니터링
- <b>`used_memory`보다 `used_memory_rss`를 보는 것이 더 중요</b>
- `used_memory` : 논리적으로 redis가 사용하는 메모리
- `used_memory_rss` : OS가 redis에 할당하기 위해 사용한 물리적 메모리 양
- 삭제되는 키가 많으면 fragmentation 증가
    - 특정 시점에 피크를 찍고 다시 삭제되는 경우
    - TTL로 인해 eviction이 많이 발생하는 경우
- 이 때 `CONFIG SET activedefrag yes` 옵션을 켜서 해결

---

## Reference
https://www.youtube.com/watch?v=92NizoBL4uA
