# 카프카 기본 개념과 구조

## 목차
1. Apache Kafka?
2. 카프카 등장 배경
3. 카프카 아키텍처
4. 카프카 동작 방식
5. 카프카 Replication
6. 카프카

# 1. Apache Kafka란?
-  실시간으로 스트림 데이터를 수집하고 처리하는 데 최적화된 ‘분산 이벤트 스트리밍 플랫폼(Distributed Data Streaming Platform)’
- 실시간으로 발생하는 대량의 데이터를 중앙 허브를 통해 흐르도록 설계되어 있음
- 이를 통해 데이터의 일관성을 유지하고 시스템 전반의 복잡성을 줄일 수 있음
- 모든 데이터는 로그 형식으로 파일 시스템에 기록
- 로그는 추가만 가능하며, 시간순으로 완전히 정렬된 데이터의 흐름(레코드 시퀀스)을 의미
- 다량의 데이터는 A 지점에서 Z지점까지의 필요한 모든 곳에 대규모 데이터를 동시에 전달할 수 있음
- 현대적인 데이터 파이프라인 구축에 필수적인 도구

![image](https://github.com/user-attachments/assets/88c9eea3-bcb8-4322-989d-2ad0f4ab47d3)


> ### 카프카가 처리하는 Stream과 Streaming
> - 스트림(Stream)이란 시간에 따라 연속적으로 발생하는 데이터의 흐름. 예를 들어 실시간 주식 값 데이터 등이 스트림의 예시
> - 스트리밍(Streaming)은 이러한 데이터 스트림을 실시간으로 처리하는 과정

> ### 분산 이벤트 스트리밍 플랫폼(Distributed Event Streaming Platform)
>
> - 여러 서버에 걸쳐 이벤트 데이터를 실시간으로 처리하고 저장하는 시스템
> - 분산 처리를 실시간으로 하고 데이터 복제를 통해 장애 상황에서도 데이터 손실 방지
> - 이벤트 발생 순서대로 처리하여 데이터의 일관성 보장

## 주요 특징
|특징|설명|
| --- | --- |
|높은 처리량|	대용량의 실시간 로그 데이터를 처리하는데 적합하며, 초당 수백만 건의 메시지를 처리 가능|
|확장 가능|	최대 1,000개의 브로커, 하루 수조 개의 메시지, 페타바이트 규모의 데이터, 수십만 개의 파티션까지 프로덕션 클러스터를 확장할 수 있으며, 저장소와 처리를 탄력적으로 확장/축소 가능|
|낮은 지연시간|	실시간 데이터 파이프라인을 구성하는데 적합한 밀리초 단위의 지연시간 제공|
|높은 가용성|	가용성 영역에 걸쳐 클러스터를 효율적으로 확장하거나 여러 지리적 영역에 걸쳐 있는 개별 클러스터 연결|
|영속성|	디스크에 데이터를 저장하여 데이터의 영속성을 보장하며, 데이터 손실을 방지|

# 2. 등장 배경
- 2011년 LinkedIn은 실시간으로 발생하는 대량의 사용자 활동 데이터(예: 프로필 조회, 메시지, 연결 요청 등)를 효율적으로 처리하고 분석해야 하는 과제에 직면
- 기존 링크드인의 데이터 처리 시스템은 각 파이프라인이 파편화되고 시스템 복잡도가 높아 새로운 시스템을 확장하기 어려운 상황이였음
- 기존 메시징 큐 시스템인 ActiveMQ를 사용했지만, 링크드인의 수많은 트래픽과 데이터를 처리하기에는 한계가 있었음
- 이로 인해 새로운 시스템의 개발 필요성이 높아졌고, 링크드인의 몇몇 개발자가 다음과 같은 목표를 가지고 새로운 시스템을 개발

![image](https://github.com/user-attachments/assets/95b5edd4-8e74-4357-bfa2-13c1fbfbba47)

- 스트림 중심의 아키텍처로 구성. 이 설정에서 Kafka는 일종의 범용 데이터 파이프라인 역할
- 카프카를 적용함으로써 모든 이벤트/데이터의 흐름을 중앙에서 관리할 수 있게 됨.
- 서비스 아키텍처가 기존에 비해 관리하기 간단해짐

![image](https://github.com/user-attachments/assets/eec8c23b-6f08-47d4-9f23-af4532911a98)

# 3. 카프카 아키텍처
![image](https://github.com/user-attachments/assets/1471f6da-fc29-4f4a-a9bf-814433bb8bcc)

### 생산자 (Producer)
- 메시지를 생산하여 브로커의 토픽으로 전달하는 역할
  - 동기/비동기 전송 지원
  - Topic에 해당하는 메세지를 생성
  - 특정 Topic으로 데이터를 publish
  - 처리 실패/재시도
    - 전송 성공 여부를 알 수 있음

### 소비자 (Consumer)
- 브로커의 토픽으로부터 저장된 메시지를 전달받는 역할
  - Topic의 partition으로부터 데이터 polling
  - Partition offset 위치 기록(commit)
  - Consumer group을 통해 병렬 처리
- Consumer의 개수는 partition의 개수보다 적거나 같아야 함
- 자동/수동 메시지 커밋

### 소비자 그룹(Consumer Group)
- 협력하여 메시지를 처리하는 소비자들의 집합
  - 자동 로드 밸런싱
  - 장애 복구 기능
  - 파티션별 할당
 
### 토픽(Topic)
- 메시지가 저장되는 논리적 단위
  - 카테고리 분류
  - 다중 구독 가능
  - 영구 저장소

### 파티션(Partition)
- 토픽을 나누는 물리적 단위
  - 병렬 처리 지원
  - 순서 보장
  - 확장성 제공

### 오프셋 (Offset)
- Consumer가 현재 읽고 있는 메시지 위치
- 파티션 내 각 메시지의 고유한 위치 식별자
  -  메시지 순서 보장
  - 소비 위치 추적
  - 재처리 가능

### 브로커(Broker)
- 카프카 애플리케이션이 설치되어 있는 서버 또는 노드를 지칭
  - 하나의 프로세스라고 생각
- 보통 3개 이상으로 구성하는 것을 권장
- 브로커의 역할
  - 컨트롤러
  - 데이터 삭제
  - 컨슈머 오프셋 저장
  - 그룹 코디네이터
  -  복제 관리
  - 장애 복구
 
### 주키퍼(Zookeeper)
- 분산 애플리케이션 관리를 위한 코디네이션 시스템
- 분산된 노드의 정보를 중앙에 집중하고 구성관리, 그룹 네이밍, 동기화 등의 서비스 수행
- 여러 개의 카프카 클러스터를 동시에 운영할 수 있음
- 카프카 3.0부터는 주키퍼가 없어도 클러스터 동작 가능 (Kraft)
  - 카프카 2.0까지는 주키퍼가 필요
 
### KRaft (Kafka Raft Metadata Mode)
- Kafka 3.0부터 도입된 주키퍼 없는 운영 모드
- Kafka 자체에서 메타데이터를 관리하는 내장 컨센서스 프로토콜 (Raft 알고리즘 기반)
- 주요 목적은 Kafka 클러스터의 의존성과 복잡성을 줄이는 것

#### KRaft의 특징
- Zookeeper 없이 Kafka 자체 컨트롤러로 메타데이터 관리
- 단일 장애 지점을 제거하여 운영 편의성 향상
- Raft를 통해 Leader 선출, 메타데이터 복제 및 일관성 유지
- 기존 Zookeeper 기반 Kafka보다 가벼운 구조
- 도입 이유
  - Zookeeper가 Kafka 클러스터의 병목이 되는 문제 제거
  - 클러스터 확장성과 장애 대응 속도 개선
  - Kafka의 단일화된 구조와 유지보수 간소화
- 현재 Kafka는 Zookeeper 기반과 KRaft 기반 둘 다 지원하며, KRaft는 향후 기본 모드로 완전 전환될 예정

# 4. 카프카 동작 방식
### 메시지 생성
- Producer는 메시지를 생성하고 특정 Topic에 전송
- 메시지는 Partition 키 또는 Round-robin 방식에 따라 적절한 Partition에 저장

### 메시지 저장
- 각 메시지는 Partition에 순차적으로 Append 되며, 디스크에 로그 형태로 저장
- 메시지는 삭제되지 않고 보존 기간(retention)이 지난 후에야 삭제

### 메시지 소비
- Consumer는 Topic의 Partition에 저장된 메시지를 Offset 기준으로 Poll 방식으로 가져옴
- 자동 커밋/수동 커밋 설정을 통해 처리 완료 여부를 저장할 수 있다.
- Consumer Group으로 묶이면 각 Partition은 그룹 내 하나의 Consumer에만 할당되어 중복 처리 방지

### 메시지 재처리
- Kafka는 메시지를 삭제하지 않기 때문에, Consumer는 필요한 경우 원하는 Offset부터 다시 읽어 재처리를 수행할 수 있다.

### 데이터 보존 정책 (Retention)
- 시간 기반: 메시지를 일정 시간(예: 7일) 동안 유지
- 용량 기반: 파티션이 설정한 용량을 초과하면 오래된 메시지 삭제
- 로그 컴팩션(Log Compaction): 최신 메시지 상태만 유지하며 나머지는 정리

# 6. 카프카 Replication (복제)

- Kafka는 고가용성과 내결함성을 위해 Partition 단위의 복제 기능을 제공
- 카프카의 replication은 토픽 자체를 복제하는 것이 아니라 토픽을 이루는 각각의 파티션을 복제함

![image](https://github.com/user-attachments/assets/de1a6771-8a1d-45d1-a63b-55f86c83d646)

Leader partition과 Follower partition을 합쳐서 ISR(In Sync Replica)

- 최대로 설정할 수 있는 replication 값은 브로커 수와 동일
- 브로커가 4개이면 replication 값은 4보다 클 수 없음

## Replication을 하는 이유
- Replication은 partition의 고가용성을 위해 사용
- 만약 replication이 1이고, partition이 1인 topic이 존재할 때, 브로커의 장애가 발생한 경우 해당 서비스를 복구할 수 없음
- 만약 replication이 2라면, 브로커 1개가 죽더라도 Follower partition이 존재하므로 복제본을 통해 복구 가능
Follower partition이 Leader partition의 역할을 승계받음

## Replication 숫자
- replication의 수가 커진다고 무조건 좋지는 않음
- replication의 수가 커진만큼 브로커의 resource 사용량이 커짐
- 따라서 카프카에 들어오는 데이터의 양과 retention date(저장 시간)을 고려해서 수를 선택하는 것이 좋음
  - broker의 수가 3개 이상일 때 replication은 3으로 설정하는 것을 권장

## Replication & ack
- 프로듀서에는 ack라는 옵션이 존재하고, 이를 통해 고가용성을 유지할 수 있음
- ack는 0, 1, all 옵션 3개 중 하나를 골라 사용 가능

### ack = 0
- 프로듀서는 Leader partition에 데이터를 전송하고 응답값을 받지 않음
- 따라서 Leader partition에 데이터가 정상적으로 전송되었는지, 나머지 partition에 정상적으로 복제되었는지 알 수 없음
- 속도는 빠르지만 데이터 유실 가능성이 있음

### ack = 1
- 프로듀서는 Leader partition에 데이터를 전송하고, Leader partition이 정상적으로 받았는지 응답값을 받음
- 하지만 나머지 partition에 정상적으로 복제되었는지 확인하지 않음
- 만약 Leader partition이 데이터를 받은 즉시 브로커에 장애가 발생하면 나머지 partition에 데이터가 아직 전송되지 않은 상태이므로 ack=0 일 때와 마찬가지로 데이터 유실 가능성이 있음

### ack = all
- 프로듀서가 Leader partition에 데이터를 전송하고, Follower partition에 복제가 잘 이루어졌는지 응답값을 받음
- ack=all을 사용하는 경우 데이터 유실은 없음
- 하지만 ack=0, 1에 비해 확인하는 부분이 많기 때문에 속도가 현저히 느림
