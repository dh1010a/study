# Transaction

## 목차
1. 트랜잭션
2. 트랜잭션의 특성
3. 트랜잭션의 상태
4. 트랜잭션 관리를 위한 DBMS의 전략
5. REDO와 UNDO
6. 트랜잭션 격리 수준
7. Deadlock
8. Statement vs PreparedStatement

# 1. 트랜잭션
- 트랜잭션이란 작업의 완전성을 보장해주는 것
- 논리적인 작업 셋을 모두 완벽하게 처리하거나 처리하지 못할 경우에는 원 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어 주는 기능
- 사용자 입장에서는 작업의 논리적 단위로 이해할 수 있음
- 시스템의 입장에서는 데이터들을 접근 또는 변경하는 프로그램의 단위
- 데이터베이스에서 트랜잭션이 필요한 이유는 데이터를 다룰 때 장애가 일어는 경우, 트랜잭션은 데이터를 복구하는 작업의 단위가 되기 때문
- 데이터베이스는 여러 작업이 동시에 같은 데이터를 다룰 떄가 있는데, 이 작업을 통해 서로 분리하고 이를 통해 오류가 발생하지 않게 함
- DBMS는 트랝개션이 이러한 규칙을 유지하도록 지원

```sql
START TRANSACTION

// (1) A 계좌 잔액 가져옴 A = 1000
// (2) B 계좌 잔액 가져옴 B = 1000

// (3) A 출금  A = A - 100
// (4) B 입금  B = B + 100
UPDATE Customer SET balance = balance - 100 WHERE name='A';
UPDATE Customer SET balance = balance + 100 WHERE name='B';

//COMMIT

// (5) A 계좌 잔액 저장 A = 900
// (6) B 계좌 잔액 저장 B = 1100

COMMIT
```

## 트랜잭션과 락
- 잠금(Lock)과 트랜잭션은 서로 비슷한 개념 같지만 잠금은 동시성을 위한 기능이고 트랜잭션은 데이터의 정합성을 보장하기 위한 기능
- 잠금은 여러 커넥션에서 동시에 동일한 자원을 요청할 경우 순서대로 한 시점에는 하나의 커넥션만 변경할 수 있게 해주는 역할
  - 여기서 자원이란 레코드나 테이블을 말함
- 이와는 조금 다르게 트랜잭션은 꼭 여러개의 변경 작업을 수행하는 쿼리가 조합되었을때만 의미 있는 개념은 아님
- 트랜잭션은 하나의 논리적인 작업 셋 중 하나의 쿼리가 있든 두 개 이상의 쿼리가 있든 관계없이 논리적인 작업 셋 자체가 100% 적용되거나 아무것도 적용되지 않아야 함을 보장하는 것
  - 예를 들어 HW 에러 또는 SW에러와 같은 문제로 인해 작업 실패시, 특별한 대책이 필요한데 이러한 문제를 해결하는 것

## 트랜잭션의 특성
- 트랜잭션은 데이터베이스의 무결성을 유지하기 위해 원자성, 일관성, 고립성, 지속성의 성질을 가짐

### Atomic(원자성)
- 트랜잭션에 포함된 작업은 전부 수행되거나 아니면 전부 수행되지 않아야 함 (All or Nothing)

### Consistency(일관성)
- 트랜잭션이 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 유지하는 것을 의미
  - 예를 들면 송금 전후 모두 잔액의 데이터 타입은 Integer이여야 한다는 것 등이 있음

### Isolation(고립성)
- 여러 트랜잭션은 동시에 수행
- 이때 각 트랜잭션은 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하여 독립적으로 작업을 수행
- 동시에 수행되는 트랜잭션이 동일한 데이터를 가지고 충돌하지 않도록 제어해주어야 함
  - 이를 동시성 제어(Concurrency Control)라고 함

 ### Durability(지속성)
 - 성공적으로 수행된 트랜잭션은 데이터베이스에 영원히 반영되어야 함
 - 트랜잭션이 완료되어 저장이 된 데이터베이스는 저장 후에 생기는 정전, 장애, 오류 등에 영향을 받지 않아야 함

![image](https://github.com/user-attachments/assets/938c0072-fcb5-4d89-ab5e-78b3785ba671)

# 3. 트랜잭션의 상태
![image](https://github.com/user-attachments/assets/b96a7f4c-39a9-44d7-9820-fbfd944a8d1b)

#### Active
- 트랜잭션의 활동 상태. 트랜잭션이 실행중이며 동작중인 상태를 말한다

#### Failed
- 트랜잭션의 실패 상태. 트랜잭션이 더이상 정상적으로 진행 할 수 없느 상태를 말한다

#### Partially Committed
- 트랜잭션의 Commit 명령이 

#### Committed
- 트랜잭션이 정상적으로 완료된 완료 상태

#### Aborted
- 트랜잭션이 취소 상태
- 트랜잭션이 취소되고 트랜잭션 실행 이전 데이터로 돌아간 상태를 말한다.

#### Partially Committed 와 Committed의 차이점
- Commit요청이 들어오면 상태는 Partially Commited 상태가 된다. 이후 커밋을 문제없이 수행할 수 있으면 Commited 상태로 전이
- 만약 오류가 발생하면 Failed 상태가 됨
- 즉, Partially Committed는 커밋 요청이 들어왔을 때를 말하며, Committed는 Commit을 정상적으로 완료한 상태를 말함

### Commit과 Rollback
- 데이터베이스는 커밋과 롤백 명령어를 통해 데이터 무결성을 보장
- 커밋이란 트랜잭션 작업을 완료했다고 확정하는 명렁어
  - 트랜잭션 작업 내용을 실제 디비에 저장하고, 디비가 변경
- 롤백은 작업 중 문제 발생시 트랜잭션 처리 과정에서 발생한 변경사항을 취소하고 이전 커밋의 상태로 되돌림

## 트랜잭션 사용시 주의점
- 트랜잭션은 꼭 필요한 최소의 코드에만 적용하는 것이 좋다. 즉, 트랜잭션의 범위를 최소화 하라는 의미
- 일반적으로 데이터베이스 커넥션은 갯수가 제한적
- 근데 각 단위 프로그램이 커넥션을 소유하는 시간이 길어진다면 사용 가능한 여유 커넥션의 갯수는 줄어들게 됨
- 그러다 어느 순간 각 단위 프로그램에서 커넥션을 가져가기 위해 기다려야하는 상황이 발생할 수 있음

## 동시성 제어 (Concurrency Control)
- 여러개의 트랜잭션이 한 개의 데이터를 동시에 갱신(Update) 할 때 어느 한 트랜잭션의 갱신이 무효화 될 수 있음
  - 이를 갱신 손실이라고 함
- 동시성 제어를 통해 갱신손실을 미리 막을 수 있음
- 즉, 트랜잭션이 동시에 수행될 때 일관성을 해치지 않도록 트랜잭션의 데이터 접근을 제어하는 DBMS의 기능을 동시성 제어라고 함
- 갱신손실 문제를 해결하기 위한 방법 중 하나로 데이터를 수정중에 있는 트랜잭션은 해당 데이터를 Lock으로 잠금장치 하여 다른 트랜잭션의 접근을 막을 수 있음
- 락이 걸린 데이터는 Unlock이 될 때 까지 다른 트랜잭션들은 접근하지 못하고 기다려야 함

 ![image](https://github.com/user-attachments/assets/12c7e332-2b4f-4d13-9194-7280905d4d24)
 
 ![image](https://github.com/user-attachments/assets/fe0c400e-c753-4caa-ae45-620b3128300c)

# 4. 트랜잭션 관리를 위한 DBMS의 전략
- 이해를 위해 알아야 할 선행 개념
  - DBMS의 구조
  - Buffer 관리 정책

### DBMS의 구조
- 크게 2가지
  - Query Processor (질의 처리기)
  - Storage System (저장 시스템)
- 데이터 저장 단위: 디스크에 데이터를 고정 길이의 Page 단위로 읽고 씀
- 저장 공간: 저장 위치: 대부분의 데이터는 **비휘발성 저장장치(disk)**에 저장되며, 필요한 일부 페이지는 **메인 메모리(Buffer Pool)**에 적재됩니다.
![image](https://github.com/user-attachments/assets/c571dc8f-4ff8-4e30-9832-5b7399364119)
 
### Buffer Manager
- DBMS의 저장 시스템에 속하는 모듈 중 하나
- **메인 메모리(Buffer Pool)**에서 관리 중인 페이지들을 관리
- 언제 어떤 Page를 디스크로 쓰기(Flush) 할지, 또는 버퍼에서 제거(Eviction) 할지 결정하는 버퍼 교체 정책을 수행함
- 이 Buffer Manager의 정책에 따라 **트랜잭션 복구 방식(UNDO / REDO)**이 달라지며, 트랜잭션 무결성에 중대한 영향을 미침
- 트랜잭션 관리에 매우 중요한 결정을 가져옴
- Storage System 내부의 핵심 모듈 중 하나


# 5. REDO와 UNDO
<img width="650" alt="image" src="https://github.com/user-attachments/assets/ae666ffb-1c78-4d87-ad00-9af8c0382088" />

- REDO는 디스크에 반영되지 않은 커밋된 데이터를 다시 적용 (-> 최신 상태로 복구)
- 디스크에 반영된 커밋되지 않은 데이터를 되돌림 (-> 이전 상태로 롤백)
- 읽기 일관성 보장을 위해 UNDO 필요
- 장애/종료 시점에 따라 REDO 또는 UNDO 수행

## UNDO
- 트랜잭션 실행 도중 변경된 Page들은 메모리 상의 Buffer Pool에 존재
- 이 Page들은 Buffer 교체 알고리즘에 의해 트랜잭션과 무관하게 디스크에 기록될 수 있음
- 커밋되지 않은 트랜잭션의 변경 사항이 디스크에 기록될 수 있으므로, 이를 원상 복구할 수 있어야 함 -> 이 작업이 UNDO
- 처리 방식
  - 트랜잭션 수행 시 변경 전의 값을 UNDO 로그에 기록
  - 장애 발생 시, 로그를 참조하여 변경 전 상태로 되돌림

### UNDO 관련 버퍼 관리 정책
- 수정된 페이지를 디스크에 쓰는 시점으로 분류

|정책	|설명	|UNDO 필요 여부|
| --- | --- | --- |
|Steal	|- 수정된 페이지를 트랜잭션 종료 전에도 디스크에 기록할 수 있음 |	필요함|
|No-Steal (¬steal)|	수정된 페이지를 트랜잭션 종료(EOT) 전까지는 디스크에 기록하지 않음|	 필요 없음|

- steal: 수정된 페이지를 언제든지 디스크에 쓸 수 있는 정책
  - 대부분 DBMS가 채택하는 버퍼 관리 정책
  - UNDO logging과 복구를 필요로 함

- no-steal : 수정된 페이지들을 EOT(End Of Transaction)까지는 버퍼에 유지하는 정책
  - UNDO작업이 필요하지 않지만, 매우 큰 메모리 버퍼가 필요

## REDO
- 커밋은 되었지만, 디스크에 반영되지 않은 트랜잭션의 변경 내용을 다시 적용하는 작업
- 시스템 장애로 인해 커밋된 변경 사항이 손실될 수 있으므로 이를 복구하여 지속성(Durability) 보장

### REDO 관련 버퍼 관리 정책
- 트랜잭션이 종료되는 시점에 해당 트랜잭션이 수정한 페이지를 디스크에 쓸 것인가 아닌가로 기준

|정책	|설명	|REDO 필요 여부|
| --- | --- | --- |
|Force	|트랜잭션 커밋 시, 수정된 모든 페이지를 디스크에 즉시 반영|	 필요 없음|
|No-Force (¬force)	|커밋 시에도 디스크에 반영하지 않고, 나중에 기록| 필요함|

- FORCE: 수정했던 모든 페이지를 트랜잭션 커밋 시점에 디스크 반영
  - 트랜잭션이 커밋 되었을 때 수정된 페이지들이 디스크 상에 반영되므로 REDO 필요 없음
- No-FORCE: 커밋 시점에 반영하지 않는 정책
  - 트랜잭션이 디스크 상의 DB에 반영되지 않을 수 있기에 REDO 복구 필요
  - 대부분의 DBMS가 채택하는 정책


# 6. 트랜잭션 격리 수준 (Transaction Isolation Level)
- Isolation Level
  - 트랜잭션에서 일관성 없는 데이터를 허용하도록 하는 수준
- Isolation level의 필요성
  - DB는 ACID 특징과 같이 트랜잭션이 독립적이 수행을 하도록 함
  - 하지만 무조건 Locking으로 동시에 수행되는 수많은 트랜잭션을 순서대로 처리하는 방식으로 구현하면 성능이 떨어짐
  - 그렇다고 성능을 고려하여 locking의 범위를 줄인다면, 잘못된 값이 처리될 수 있는 문제 발생
    - 최대한 효율적인 Locking방법이 필요

 ## Isolation level 종류
 - 격리 수준이 높아질수록 일관성은 보장되지만 동시성(성능)은 낮아짐

 ### 1. Read Uncommitted (Lv. 0)
> SELECT 문장이 수행되는 동안 Shared Lock이 걸리지 않는 가장 낮은 격리 수준

- 다른 트랜잭션의 미커밋된 데이터도 읽을 수 있음
  - Dirty Read 발생 가능
  - 사용자 1이 A 데이터를 B로 변경하는 동안 사용자 2는 아직 완료되지 않은(Uncommitted) 트랜잭션이지만 B를 읽을 수 있음
- 트랜잭션 중간 결과를 다른 트랜잭션이 볼 수 있어 데이터 일관성 보장 불가
- 락을 거의 사용하지 않기 때문에 성능은 가장 좋음
- 실무에서는 거의 사용되지 않음

### 2. Read Committed (Lv. 1)
> SELECT 문장이 수행되는 동안 Shared Lock이 걸리며, 오직 커밋된 데이터만 읽을 수 있는 수준

- 트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기
- Commit이 이루어진 트랜잭션만 조회 가능
- 대부분 SQL 서버가 기본으로 사용하는 격리 수준
  - 사용자1이 A라는 데이터를 B로 변경하는 동안 사용자 2는 해당 데이터에 접근 불가능
- Dirty Read는 방지됨하지만, 트랜잭션 도중 같은 데이터를 다시 조회했을 때 값이 바뀔 수 있음
  - Non-Repeatable Read 발생 가능
  - 사용자 1이 A 데이터를 수정하고 커밋함
  - 사용자 2는 그 이전과 이후 조회 시 서로 다른 결과를 볼 수 있음



### 3. Repeatable Read (Lv. 2)
> 트랜잭션이 끝날 때까지 조회한 모든 데이터에 Shared Lock을 유지하는 격리 수준

- 트랜잭션이 범위 내에서 조회한 데이터 내용이 항상 동일함을 보장
- 다른 사용자는 트랜잭션 영역이 해당되는 데이터에 대한 수정 불가능
- MySQL, InnoDB의 에서 기본으로 사용하는 격리 수준
  - InnoDB는 Gap Lock을 통해 팬텀 리드도 일부 방지
- Dirty Read, Non-Repeatable Read 방지
- 하지만, 다른 트랜잭션이 새로운 행을 삽입하면, 기존 조건으로 조회했을 때 결과가 달라질 수 있음
  - Phantom Read 발생 가능
  - 사용자 1이 A~Z 범위의 데이터를 조회 중
  - 사용자 2가 X 값을 새로 삽입하면, 사용자 1은 동일 쿼리로 다시 조회 시 결과가 달라질 수 있음

### 4. Serializable (Lv. 3)
> 모든 트랜잭션이 직렬적으로 실행되는 것처럼 동작하는 가장 엄격한 격리 수준

- 완벽한 읽기 일관성 모드 제공
- 다른 사용자는 트랜잭션 영역에 해당되는 데이터에 대한 수정 및 입력 불가능
- Dirty Read, Non-Repeatable Read, Phantom Read 전부 방지
- SELECT 시점의 데이터 뿐만 아니라, 해당 조건에 걸릴 수 있는 신규 데이터의 삽입도 제한
- 동시성 성능 저하가 가장 심함, 락 충돌 가능성 많음
  - 사용자 1이 "서울 지역 고객"을 조회 중
  - 사용자 2는 새로운 서울 고객 등록이 불가능 -> 직렬성 보장

### 선택시 고려사항
- Isolation Level에 대한 조정은, 동시성과 데이터 무결성에 연관됨
- 동시성을 증가시키면 데이터 무결성에 문제 발생
- 데이터 무결성을 유지하면 동시성이 떨어지게 됨
- 네벨을 높게 조정할 수록 발생하는 비용 증가

## 낮은 단계 Isolation 사용시 발생하는 현상들
### Dirty Read
- 커밋되지 않은 수정중인 데이터를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생하는 현상
- 어떤 트랜잭션에서 아직 실행이 끝나지 않은 다른 트랜잭션에 의한 변경사항을 보게되는 경우

### Non-Repeatable Read
- 한 트랜잭션에서 같은 쿼리를 두 번 수행할 때 그 사이에 다른 트랜잭션 값을 수정 또는 삭제하면서 두 쿼리의 결과가 상이하게 나타나는 일관성이 깨진 현상

### Phantom Read
- 한 트랜잭션 안에서 일정 범위의 레코드를 두 번 이상 읽었을 때, 첫번째 쿼리에서 없던 레코드가 두번째 쿼리에서 나타나는 현상
- 트랜잭션 도중 새로운 레코드 삽입을 허용하기 때문에 나타나는 현상


### 정리

|격리 수준	|Dirty Read|	Non-Repeatable Read	|Phantom Read	|성능	|대표 사용 DB|
| --- | --- | --- | --- | --- | --- |
|Read Uncommitted	|발생함	|발생함	| 발생함	| 매우 빠름	|거의 사용 안함|
|Read Committed	|방지	| 발생함	| 발생함	| 일반적	|Oracle, PostgreSQL|
|Repeatable Read	| 방지	| 방지	| 발생함	| 균형	|MySQL (기본)|
|Serializable	| 방지	| 방지	| 방지	| 느림	| 특수한 경우|

# 7. Deadlock
- 여러 트랜잭션이 각각 Lock을 설정하고, Unlock을 하지 않은 상태에서 서로의 락이 걸린 데이터에 접근하려고 할 때, 서로 대기를 계속하여 영원히 처리되지 않는 상황이 발생

### 교착 상태의 빈도를 낮추는 방법
- 트랜잭션을 자주 커밋한다.
- 정해진 순서로 테이블에 접근.
  - 위에서 트랜잭션 1 이 테이블 B -> A 의 순으로 접근했고, 트랜잭션 2 는 테이블 A -> B의 순으로 접근
  - 트랜잭션들이 동일한 테이블 순으로 접근하게 한다.
- 읽기 잠금 획득 (SELECT ~ FOR UPDATE)의 사용을 피한다
- 한 테이블의 복수 행을 복수의 연결에서 순서 없이 갱신하면 교착상태가 발생하기 쉬움
  - 이 경우에는 테이블 단위의 잠금을 획득해 갱신을 직렬화 하면 동시성은 떨어지지만 교착상태를 회피할 수 있다.

### deadlock을 해결하는 방법

#### 예방기법
- 각 transaction이 실행되기 전에 필요한 데이터를 모두 Locking 해주는 것
- locking해줘야 하는 데이터가 많다면 사실상 모든 데이터를 전부 locking한 것과 동일하여 transaction의 병행성을 보장하지 못할 수 있음
  
#### 회피기법
- 자원을 할당할 때 timestamp를 사용하여 deadlock가 일어나지 않도록 회피하는 방법
- deadlock을 예방하기 위해 트랜잭션이 시작된 시간을 timestamp로 삼고, 이를 기준으로 크게 두 방식으로 처리

1. Wait-die
  - T1, T2 두 개의 transaction이 있을 경우, T2가 선점하고 있는 data에 T1이 접근을 요청
  - T1의 timestamp가 T2의 timestamp보다 더 older일 경우에는 wait 할 수 있음
  - 그렇지 않은 경우에 T1은 die (roll back)
2. Wound-wait
  - T2가 선점하고 있는 data에 T1이 접근을 요청
  - T1의 timestamp가 T2의 timestamp보다 더 younger일 경우에는 wait 가능
  - 그렇지 않은 경우에 T2는 die(roll back)
  
#### 탐지/회복 기법
- Transaction이 실행되기 전에는 아무런 검사를 하지 않고, deadlock이 발생하면 이를 감지하고 회복시키는 방법

![image](https://github.com/user-attachments/assets/a933cf8d-455f-4017-b0ef-7c5f318bc688)


# 8. Statement vs PreparedStatement

## Statement
- 가장 기본적인 SQL 실행 도구
- 정적인 SQL을 실행할 때 사용
- SQL 구문을 문자열로 직접 작성해서 실행

```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM users WHERE id = " + userId);
```

### 문제점
- SQL 구문이 매번 서버에서 재컴파일
- SQL Injection 공격에 취약
- 반복 수행 시 비효율적 (매번 파싱 + 컴파일)

## PreparedStatement
- 동적 SQL을 처리할 수 있는 SQL 실행 도구
- ? 로 값을 바인딩하고, 쿼리를 미리 컴파일(Pre-compile) 해둠

```java
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
pstmt.setInt(1, userId);
ResultSet rs = pstmt.executeQuery();
```

### 장점
- 성능 향상: 쿼리를 미리 컴파일해두기 때문에, 반복 실행 시 빠름
- 보안 강화: 입력값이 자동으로 이스케이프 처리되어 SQL Injection 방지
- 가독성/유지보수성도 좋아짐

우선 속도 면에서 `PreparedStatement`가 빠르다고 알려져 있다. 이유는 쿼리를 수행하기 전에 이미 쿼리가 컴파일 되어 있으며, 반복 수행의 경우 프리 컴파일된 쿼리를 통해 수행이 이루어지기 때문

`Statement`에는 보통 변수를 설정하고 바인딩하는 static sql이 사용되고 `Prepared Statement`에서는 쿼리 자체에 조건이 들어가는 dynamic sql이 사용된다. `PreparedStatement`가 파싱 타임을 줄여주는 것은 분명하지만 dynamic sql을 사용하는데 따르는 퍼포먼스 저하를 고려하지 않을 수 없다.

하지만 성능을 고려할 때 시간 부분에서 가장 큰 비중을 차지하는 것은 **테이블에서 레코드(row)를 가져오는 과정**이고 SQL 문을 파싱하는 시간은 이 시간의 10 분의 1 에 불과하다. 그렇기 때문에 `SQL Injection` 등의 문제를 보완해주는 `PreparedStatement`를 사용하는 것이 옳다.

## 정리
- PreparedStatement가 항상 무조건 더 빠르다고 단정하긴 어려움
- 쿼리 파싱/컴파일 시간은 전체 SQL 수행 시간 중 극히 일부분
- 대부분의 SQL 실행 시간은 디스크 I/O(=레코드 조회) 에서 발생
- 그럼에도 불구하고 `PreparedStatement`는
  - SQL Injection 방지
  - 재사용 가능성 높음
  - DB 캐시 사용 가능
- 이런 이유들로 **"성능보다 보안과 안정성에서 큰 이점"**을 제공하므로, 실무에서는 권장

<img width="639" alt="image" src="https://github.com/user-attachments/assets/d5f36c76-2272-4913-985b-34c344f834be" />


## Ref
- [tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/Transaction%20Isolation%20Level.md)
- [Interview_Question_for_Beginner](https://github.com/jbee37142/Interview_Question_for_Beginner/tree/main/Database)
- [기출로 대비하는 개발자 전공면접](https://www.inflearn.com/course/%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%A0%84%EA%B3%B5%EB%A9%B4%EC%A0%91-cs-%EC%99%84%EC%A0%84%EC%A0%95%EB%B3%B5/dashboard)
