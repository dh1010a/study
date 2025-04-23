# PostgreSQL

## 목차
1. PostgreSQL이란?
2. PostgreSQL의 기능
3. PostgreSQL의 특징
4. 다른 DB와의 비교

# 1. PostgreSQL이란?
- 오픈 소스 객체-관계형 데이터베이스 시스템(ORDBMS)
- 엔터프라이즈급 DBMS의 기능과 여러 기능을 제공함
- 다른 관계형 데이터베이스 시스템과 달리 연산자, 복합 자료형, 집게 함수, 자료형 변환자, 확장 기능 등 다양한 데이터베이스 객체를 사용자가 임의로 만들 수 있는 기능을 제공
- 이러한 유연성 덕분에 단순한 RDBMS를 넘어, 마치 하나의 데이터 처리 언어/플랫폼처럼 다양한 기능을 구현할 수 있음

## PostgreSQL의 구조
- PostgreSQL은 클라이언트/서버 모델을 사용
- 서버는 데이터베이스 파일들을 관리하며, 클라이언트 애플리케이션으로 부터 들어오는 연결을 수용하고 클라이언트를 대신하여 데이터베이스 액션을 수행
- 서버는 다중 클라이언트 연결을 처리할 수 있음
- 서버는 클라이언트의 연결 요청이 오면 각 커넥션에 대해 새로운 프로세스를 fork()함
  - Backend Process
  - 스레드가 아니라 프로세스 기반 구조라 메모리 격리성이 높음
- 클라이언트는 해당 Backend Process와 직접 통신하며, 서버의 다른 커넥션과 충돌 없이 작업을 수행할 수 있음

![image](https://github.com/user-attachments/assets/e547a70a-ba1e-47a2-8d84-1d395fabd719)

> 참고: PostgreSQL의 내부 구조는 Parser → Rewriter → Planner/Optimizer → Executor → Buffer → Storage 흐름을 따르며, Write-Ahead Logging(WAL)과 MVCC 기반 동시성 제어도 함께 동작함

# 2. PostgreSQL의 기능
- 관계형 DBMS의 기본적인 기능인 트랜잭션과 ACID를 지원
- 표준 SQL 기능을 가장 많이 지원함
- PostgreSQL은 기본적인 신뢰도와 안정성을 위한 기능 뿐 아니라 진보적인 기능이나 연구를 위한 확장 기능도 많음

### 고급 SQL 기능
- PostgreSQL은 ANSI SQL 표준을 가장 충실히 따르는 DBMS 중 하나로, 고급 SQL문법을 폭넓게 지원
- CTE(Command Table Expression), 재귀 쿼리(WITH RECURSIVE)
- 윈도우 함수 (ROW_NUMBER, RANK, SUM OVER)
- Partial Index, Expression Index, 함수 기반 인덱싱
- 트랜잭션 격리 수준 세부 조정 가능

### JSON/비정형 데이터 처리
- JSON 및 JSONB 타입을 통해 문서 기반 데이터도 효율적 처리 가능
- JSONB는 이진 포맷으로 저장되어 인덱싱 및 필터링이 빠름
- ->, ->>, @>, #>> 등의 연산자를 통한 JSON 탐색
- GIN 인덱스를 활용해 복잡한 JSON 구조도 성능 저하 없이 검색 가능

### MVCC 기반 동시성 제어
- PostgreSQL은 MVCC 방식을 사용해 락 없이 동시에 여러 트랜잭션을 처리할 수 있음
- 각 트랜잭션은 자신만의 스냅샷을 기반으로 쿼리를 수행하므로, 일관된 결과를 보장하면서도 높은 동시성 유지

### 확장성과 사용자 정의 기능
- 사용자 정의: 함수(Function), 타입(Type), 연산자(Operator), 인덱스 연산자 클래스
- PostgreSQL은 다양한 언어로 함수 작성 가능: PL/pgSQL, PL/Python, PL/Perl 등
- 확장 기능(Extension)을 쉽게 설치 가능: PostGIS, pg_stat_statements, uuid-ossp, pgcrypto 등

### 안정성과 복제 기능
- Write-Ahead Logging(WAL) 기반 장애 복구
- Streaming Replication, Logical Replication, Hot Standby 등 고급 복제 지원
- 복제를 통한 고가용성(HA) 구성이 쉬움
- Point-In-Time Recovery(PITR) 로 장애 시 특정 시점으로 복원 가능

### 성능 최적화 도구
- 실행 계획 분석 도구: EXPLAIN, EXPLAIN ANALYZE
- 통계 기반 최적화: ANALYZE, pg_stat_* 테이블
- 자동/수동 VACUUM, ANALYZE, Autovacuum Daemon
- 고급 인덱스: GIN, GiST, BRIN, SP-GiST 등

### 다양한 데이터 타입과 제약 조건
- 지원 데이터 타입: ARRAY, UUID, INET, TSVECTOR, JSONB 등
- 제약 조건: CHECK, UNIQUE, FOREIGN KEY, EXCLUSION CONSTRAINT (지리적 범위 등에서 유용)

이 외에도 더 많은 기능이 존재

# 3. Postgre의 특징
PostgreSQL은 현대적인 애플리케이션에서 요구하는 다양한 요구사항을 충족할 수 있는 확장성과 유연성을 갖춘 객체-관계형 데이터베이스(ORDBMS)

### 표준 SQL과 호환성이 높음
- PostgreSQL은 ANSI SQL 표준을 거의 완벽하게 지원하는 DBMS 중 하나로, CTE(Common Table Expression), 윈도우 함수, 서브쿼리, 복합 조인 등 복잡한 SQL 문법을 자연스럽게 사용할 수 있음
- 이를 통해 가독성이 높고 유지보수가 쉬운 쿼리 작성이 가능하며, 데이터 분석과 집계 로직을 SQL 단에서 효율적으로 처리할 수 있음

### 강력한 트랜잭션 및 MVCC
- PostgreSQL은 ACID(원자성, 일관성, 격리성, 지속성) 를 철저히 지키는 트랜잭션을 지원
- 또한 MVCC 기반으로 동시성을 처리하기 때문에, 하나의 데이터에 대해 다수의 트랜잭션이 동시에 접근하더라도 락 충돌 없이 안정적인 처리와 높은 성능을 동시에 보장

### JSON, 배열, 비정형 데이터 처리에 강함
- 관계형 데이터베이스임에도 불구하고, PostgreSQL은 JSON 및 JSONB 타입을 통해 문서 기반 비정형 데이터도 자유롭게 다룰 수 있음
- 이외에도 배열(Array), hstore(키-값 저장), XML 등 다양한 비정형 데이터 타입을 제공하며, 이들에 대한 인덱싱과 검색 기능까지 갖추고 있음
- 특히 GIN 인덱스를 사용하면 JSON 내부 구조에 대해 빠르게 필터링할 수 있어, NoSQL의 유연성과 RDB의 안정성을 동시에 얻을 수 있음

> #### GIN 인덱스
>
> - 하나의 컬럼 안에 여러 값을 가질 수 있는 자료형에 대해, 그 내부 요소를 기준으로 검색 성능을 높이기 위한 인덱스
> - GIN은 역색인(Inverted Index) 방식
> - 값 -> 위치 로 매핑하는 구조
> - 반대로 일반 B-Tree는 위치 -> 값 으로 탐색
> - PostgreSQL에서 JSONB나 배열처럼 컬럼 하나에 여러 요소가 들어 있는 자료형을 검색할 때는 GIN 인덱스를 활용
> - 예를 들어, JSON 문서 안에 특정 key-value가 포함돼 있는지를 빠르게 필터링할 수 있어, 로그 데이터나 설정 정보 검색 등에 유리

### 다양한 인덱스 및 성능 최적화 기능
- PostgreSQL은 기본 B-Tree 인덱스 외에도 GIN, GiST, BRIN, SP-GiST 등 다양한 인덱스 방식을 지원
- 또한 Partial Index, Expression Index, 함수 기반 인덱스 등 실무 상황에 맞는 유연한 인덱스 전략이 가능
- EXPLAIN, ANALYZE 명령어를 활용하면 쿼리의 실행 계획을 시각화해 성능 병목 지점을 분석할 수 있고, 통계 수집(ANALYZE)과 자동 튜닝도 지원

### 뛰어난 확장성과 유연한 커스터마이징
- PostgreSQL은 사용자 정의 함수(Function), 자료형(Type), 연산자(Operator), 인덱스 연산자 클래스 등을 지원하여 매우 높은 수준의 커스터마이징이 가능
- 다양한 외부 확장 기능(Extension)도 쉽게 설치해 사용할 수 있다. 대표적으로 지리 정보 처리 확장인 PostGIS, 시계열 데이터 확장인 TimescaleDB, 통계 분석 도구 pg_stat_statements 등

### 강력한 복제 및 고가용성 기능
- PostgreSQL은 Write-Ahead Logging(WAL) 을 기반으로, 장애 시 빠른 복구를 가능하게 하고, 복제를 통한 고가용성(HA)을 지원
- Streaming Replication, Logical Replication, Hot Standby를 이용해 다양한 아키텍처에 맞는 복제 구성이 가능하며, Point-In-Time Recovery(PITR) 를 통해 장애 발생 시 특정 시점으로 복구할 수 있음

### Template 데이터베이스
- PostgreSQL에서 데이터베이스 생성시 기본으로 생성되어 있는 Template1 Database를 복사하여 생성
- 템플릿 데이터베이스는 표준 시스템 데이터베이스로 원본 데이터베이스에 해당
- 만약 템플릿1에서 프로시저 언어 PL/Perl을 설치하는 경우 해당 데이터베이스를 생성할 때 추가 작업 없이 사용자 데이터베이스가 자동으로 사용 가능
- PostgresQL에는 Template0라는 2차 표준 시스템 데이터베이스가 있는데, 이 데이터베이스에는 template1의 초기 내용과 동일한 데이터가 포함되어 있음
- Template 0은 수정하지 않고 원본 그대로 유지하여 무수정 상태의 데이터베이스를 생성할 수 있으며, pg_dump 덤프를 복원할 때 유용하게 사용할 수 있음
- 일반적으로 template1에는 인코딩이나 로케일 등과 같은 설정들을 해주고, 템플릿을 복사하여 데이터베이스를 생성한
- template0을 통해서는 새로운 인코딩 및 로케일 설정을 지정할 수 있다.

### Vacuum
- Vacuum은 PostgreSQL에만 존재하는 고유 명령어로, 오래된 영역을 재사용하거나 정리해주는 명령어
- PostgreSQL 에서는 MVCC 기법을 활용하기 때문에 특정 ROW를 추가 또는 업데이트 할 경우, 디스크 상의 해당 Row를 물리적으로 업데이트 하여 사용하지 않고 새로운 영역을 사용
- 전체 테이블을 업데이트 하는 경우 자료의 수만큼 자료 공간이 늘어나게 됨
- 그러므로 수정,삭제,삽입이 자주 일어나는 데이터베이스의 경우는 물리적인 저장 공간이 삭제되지 않고 남아있게 되므로 베큠을 주기적으로 해주는 것이 좋음
- 베큠을 사용하면 어느 곳에서도 참조되지 않고, 안전하게 재사용할 수 있는 행을 찾아 FSM(Free Space Map)이라는 메모리 공간에 그 위치와 크기를 기록
- 삽입 및 업데이트 등 새로운 행을 추가하는 경우, FSM에서 새로운 데이터를 저장할 수 있는 적당한 크기의 행을 찾아 사용

# 4. 다른 DB와의 비교

|항목 | PostgreSQL | MySQL | Oracle | SQL Server | MongoDB|
| --- | --- | --- | --- | --- | --- |
|유형 | 객체-관계형 | 관계형 | 상용 관계형 | 상용 관계형 | NoSQL (문서형)
|라이선스 | 오픈소스 (자유로운 사용) | 오픈소스 (GPL) | 상용 (유료) | 상용 (유료) | 오픈소스 (SSPL)
|표준 SQL 지원 | 매우 강력 | 제한적 | 매우 강력 | 매우 강력 | 없음
|트랜잭션 | MVCC 기반 | InnoDB에서 지원 | 강력한 트랜잭션 관리 | 완전 지원 | 제한적
|JSON 처리 | JSON/JSONB + 고급 인덱스 | 지원은 되지만 기능 제한 | 제한적 | 일부 지원 | 문서 기반 구조
|확장성 | 사용자 정의 타입, 함수 등 고급 커스터마이징 가능 | 제한적 | 제한적 | 제한적 | 뛰어난 수평 확장
|복제 및 고가용성 | 스트리밍/논리 복제, PITR | 쉬운 복제 | 고급 기능 제공 (RAC 등) | AlwaysOn 등 | 샤딩 구조|
|인덱스 종류 | B-Tree, GIN, GiST 등 다양 | 기본 인덱스 위주 | B-Tree, Bitmap 등 | 다양 | 자체 방식|
|강점 | 확장성, 표준 준수, 복잡 쿼리 처리 | 속도, 단순성 | 안정성과 고성능 | BI 연동 강점 | 유연한 구조, 빠른 개발
|약점 | 삽입 성능, 설정 복잡도 | 고급 기능 부족 | 비용, 유연성 낮음 | 비용, 플랫폼 종속 | 트랜잭션 미흡


### PostgreSQL의 강점
- PostgreSQL이 특히 복잡한 데이터 구조와 고급 쿼리가 필요한 시스템에서 강점을 가짐
- PostgreSQL은 표준 SQL을 매우 잘 지원하기 때문에 CTE, 윈도우 함수, 재귀 쿼리 등 복잡한 로직을 SQL 레벨에서 깔끔하게 처리할 수 있음
- JSONB나 배열 같은 비정형 데이터도 효율적으로 다룰 수 있고, GIN 인덱스를 통해 성능 저하 없이 빠르게 검색할 수 있다는 점이 실무에서 유용
- MVCC 기반의 트랜잭션 처리와 동시성 제어가 강력해서, 데이터 정합성이 중요한 서비스에도 잘 맞음
- 확장성과 커스터마이징 측면에서도 사용자 정의 타입이나 함수, 인덱스를 자유롭게 구성할 수 있어 복잡한 도메인 로직을 데이터베이스 레벨에서 잘 분리하고 유지할 수 있다는 점도 PostgreSQL을 선택하는 이유 중 하나



