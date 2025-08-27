
## 1. Snowflake란?

- Snowflake는 **클라우드 기반 데이터 웨어하우스(DWH, Data Warehouse)**
- 기존의 온프레미스 DWH(예: Oracle Exadata, Teradata)와 달리 클라우드 네이티브 아키텍처로 설계 됨
- AWS, Azure, GCP 등 다양한 클라우드 환경에서 동일하게 사용 가능
- 즉, 인프라 관리보다는 데이터 저장·처리·분석에 집중할 수 있도록 지원하는 서비스형 데이터 웨어하우스(SaaS)라고 할 수 있음

## 2. Snowflake 아키텍처

- Snowflake의 핵심은 스토리지와 컴퓨팅의 분리
- Storage Layer: 데이터는 자동 압축 및 컬럼 기반 저장 방식으로 관리 됨
- Compute Layer (Virtual Warehouse): 쿼리를 실행하는 컴퓨팅 리소스로, 필요할 때만 활성화하여 사용할 수 있으며 동시에 여러 팀이 독립적으로 운영 가능
- Cloud Services Layer: 인증, 보안, 쿼리 최적화, 스케줄링 등 관리 기능을 담당

## 3. Snowflake의 주요 장점

- 높은 확장성
  - 필요에 따라 컴퓨팅 리소스를 즉시 늘리거나 줄일 수 있으며, 여러 사용자가 동시에 접속해도 성능 저하가 적음
- 운영 편의성
  - 인덱스 관리, 파티셔닝, 서버 관리 등의 작업이 필요하지 않고 SQL만 작성하면 됨
- 멀티 클라우드 지원
  - AWS, Azure, GCP 어디서나 동일한 경험을 제공하여 특정 클라우드 벤더 종속성이 줄어듦

## 4. Snowflake의 특화 기능

- Time Travel: 특정 시점의 데이터를 조회할 수 있어 데이터 복구와 변경 추적이 용이
- Zero-Copy Cloning: 실제 데이터를 복사하지 않고 메타데이터 레벨에서 즉시 복제할 수 있어 비용과 시간을 절약할 수 있음
- Semi-Structured Data 지원: JSON, Avro, Parquet 등 반정형 데이터도 테이블처럼 쿼리할 수 있음
- 보안 및 거버넌스: 자동 암호화, Role-Based Access Control, 데이터 마스킹 등의 기능을 기본 제공하여 보안 수준이 높음

## 5. Snowflake SQL의 특징

- Snowflake는 ANSI SQL을 준수하면서도 몇 가지 확장 기능을 제공
- 대표적인 예로 QUALIFY 절이 있음. 이는 윈도우 함수 결과를 필터링하기 위한 전용 절로, 기존 DB에서는 서브쿼리나 CTE를 사용해야 했던 작업을 간단히 처리할 수 있도록 지원
