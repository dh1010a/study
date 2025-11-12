# Index

## 목차
1. Index란?
2. Index 구조
3. Clustered Index vs Non-clustered Index
4. Index 장단점
5. 인덱스 효과적으로 사용하는법

# 1. Index란?
- 추가적인 쓰기 작업과 저장 공간을 활용하여 데이터베이스 테이블의 검색 속도를 향상시키기 위한 자료구조
- 테이블의 칼럼을 색인화 함
- 데이터베이스 안의 레코드를 full table scan 하지 않고, B+ Tree로 구성된 구조에서 인덱스 파일 검색으로 속도를 향상시키는 기술
- DBMS의 인덱스는 항상 정렬된 상태를 유지
  - 원하는 값을 탐색하는데 빠르지만 새로운 값을 추가하거나 삭제, 수정하는 경우 쿼리문 실행 속도가 느려짐
- 결론적으로 DBMS에서 인덱스는 저장 성능을 희생하고 그 대신 데이터의 읽기 속도를 높이는 기능
- SELECT 쿼리 문장의 WHERE에 사용된다고 전부 인덱스로 생성하면 저장 성능이 커지고 인덱스가 커져 역효과가 남

## 파일 구성
- 테이블 생성 시, 3가지 파일이 생성
  - FRM : 테이블 구조 저장 파일
  - MYD : 실제 데이터 파일
  - MYI : Index 정보 파일 (Index 사용 시 생성)

사용자가 쿼리를 통해 Index를 사용하는 칼럼을 검색하게 되면, 이때 MYI 파일의 내용을 활용

## Index를 사용하는 이유
- Table에 데이터를 지속적으로 저장하면 내부적으로 순서없이 쌓임
- 조회시 Where절을 사용하면 Table의 row(record)를 처음부터 끝까지 모두 접근하여 검색조건과 일치하는지 비교하는 과정 필요 (Full table scan)
- 특정 column에 대한 Index를 생성해놓은 경우 해당 속성에 대한 search-key가 정렬되어 저장되어 있어 조건 검색 (Select ~ where) 속도가 매우 빠름

# 2. Index 구조
- 인덱스는 B tree, B+ tree, Hash, Bitmap으로 구현 가능
- 많은 데이터베이스 시스템에서 인덱스는 B+ Tree 구조를 가짐
- Index 를생성하게 되면 특정 Column(속성, attribute)의 값을 기준으로 정렬하여 데이터의 물리적 위치와 함께 별도 파일 저장
  - 이때 인덱스에 저장되는 속성 값을 search-key값이라고 함
  - 실제 데이터의 물리적 위치를 저장한 값을 pointer라고 함
- 그래서 인덱스는 정렬된 검색키와 포인터만 저장하여 테이블보다 적은 공간 차지 (보통 테이블의 10% 정도)

> 특정 column을 search-key 값을 설정하여 index를 생성하면, 해당 search-key 값을 기준으로 정렬하여 (search-key, pointer)를 별도 파일에 저장. 이를 인덱스라고 함

### B-Tree 자료구조
- 이진 탐색 트리와 유사한 자료구조
- 자식 노드를 둘 이상 가질 수 있고 Balanced Tree 라는 특징이 있음
  - 즉 탐색 연산에 있어 O(log N)의 시간 복잡도를 가짐
- 모든 노드들에 대해 값을 저장하고 있으며 포인터 역할을 동반

### B+Tree 자료구조
- B-Tree를 개선한 형태의 자료구조
- 값을 리프노드에만 저장하며 리프노드들 끼리는 Linked List로 연결되어 있음
  - 때문에 부등호문 연산에 대해 효과적
- 리프 노드를 제외한 노드들은 포인터의 역할만을 수행

### HashTable 자료구조
- 해시 함수를 이용해서 값을 인덱스로 변경하여 관리하는 자료구조
- 일반적인 경우 탐색,삽입,삭제 연산에 대해 O(1)의 시간 복잡도를 가짐
- 다른 관리 방식에 비해 빠른 성능을 가짐
- 최악의 경우 해시 충돌이 발생하는 것으로 탐색, 삽입, 삭제 연산에 대해 O(N)의 시간 복잡도를 가짐

## 해쉬가 빠른 성능을 가지지만, B+ 트리를 사용하는 이유
- 하나의 값만 가져올 때는 해시 테이블은 O(1)의 시간으로 B+ 트리보다 빠름
- 값이 정렬되어 있지 않기 때문에 부등호 쿼리에 대해서는 비효율적
- B+트리는 항상 정렬된 상태를 유지하여 부등호 연산에 유리
- 데이터의 탐색 뿐 아니라 저장, 수정, 삭제에도 항상 O(long N)의 시간 복잡도를 가짐

![image](https://github.com/user-attachments/assets/c0b7ceda-7d98-4895-9b79-57a127fac9cf)

![image](https://github.com/user-attachments/assets/1a2b08a1-76e2-4281-957c-a6ae6d17262b)

![image](https://github.com/user-attachments/assets/f3faab23-a701-438f-96a0-04341cf9ae09)

- hash index는 빠른 데이터 검색 O(1)이 필요할 때 유용
- 하지만 index로써 hash index가 사용되는 경우는 제한적
- 왜냐하면 hash index는 등호(=) 연산에만 특화됨
- 데이터가 조금이라도 달라지면 hash function은 완전히 다른 hash 값을 생성하는데, 이러한 특성 때문에 부등호 연산(>, <)이 자주 사용되는 DB 검색에는 hash index가 적합하지 않음

## DML이 일어날때의 상황

### Insert
- 기존 Block에 여유가 없을 때, 새로운 Data가 입력
- 새로운 Block을 할당 받은 후, Key를 옮기는 작업을 수행
- Index split 작업 동안, 해당 Block의 Key 값에 대해서 DML이 블로킹 (대기 이벤트 발생)
- 이때 Block의 논리적인 순서와 물리적인 순서가 달라질 수 있음 (인덱스 조각화)

### DELETE
<Table과 Index 상황 비교>
- Table에서 data가 delete 되는 경우 : Data가 지워지고, 다른 Data가 그 공간을 사용 가능
- Index에서 Data가 delete 되는 경우 : Data가 지워지지 않고, 사용 안 됨 표시만 해둠
  - Table의 Data 수와 Index의 Data 수가 다를 수 있음

## UPDATE
- Table에서 update가 발생 -> Index는 Update 할 수 없음
- Index에서는 Delete가 발생한 후, 새로운 작업의 Insert 작업 / 2배의 작업(DELETE + INSERT)이 소요


# 3. Clustered Index vs Non-clustered Index

## Clustering Index: 
- 특정 Column를 기본키(primary key)로 지정하면 자동으로 클러스터형 인덱스가 생성되고 해당 colums 기준으로 정렬됨
- table 자체가 하나의 인덱스
- 클러스터(Cluster)란 여러 개를 하나로 묶는다는 의미로 주로 사용되는데, 클러스터드 인덱스도 크게 다르지 않음
- 인덱스에서 클러스터드는 비슷한 것들을 묶어서 저장하는 형태로 구현
- 이는 주로 비슷한 값들을 동시에 조회하는 경우가 많다는 점에서 착안된 것
- 여기서 비슷한 값들은 물리적으로 인접한 장소에 저장 되어 있는 데이터들을 말함

- 클러스터드 인덱스(Clustered Index)는 인덱스가 적용된 속성 값에 의해 레코드의 물리적 저장 위치가 결정되는 인덱스
- 일반적으로 데이터베이스 시스템은 Primary Key에 대해서 기본적으로 클러스터드 인덱스를 생성한다.
  - Primary Key는 행마다 고유하며 Null 값을 가질 수 없기 때문에 물리적 정렬 기준으로 적합
  - 이러한 경우 Primary Key 값이 비슷한 레코드끼리 묶어서 저장
- 물론 Primary Key가 아닌 다른 칼럼에 대해서도 클러스터드 인덱스를 생성할 수 있음
- 클러스터드 인덱스에서는 인덱스가 적용된 속성 값(주로 Primary Key)에 의해 레코드의 저장 위치가 결정 되며 속성 값이 변경되면 그 레코드의 물리적인 저장 위치 또한 변경
  - 어떤 속성에 클러스터드 인덱스를 적용할지 신중하게 결정하고 사용
- 클러스터드 인덱스는 테이블 당 한 개만 생성 가능

### Non-Clustered Index
- 논클러스터드 인덱스(Non-clustered Index)는 데이터를 물리적으로 정렬하지 않음
- 논클러스터드 인덱스는 별도의 인덱스 테이블을 만들어 실제 데이터 테이블의 행을 참조
- 테이블 당 여러 개의 논클러스터드 인덱스를 생성할 수 있음

### Secondary index
- 일반 책의 찾아보기와 같이 별도 공간에 인덱스 생성
- create index와 같이 인덱스를 생성하기를 하거나 고유키 (unique key)로 지정하면 보조 인덱스 생성

 ### Primary Index
- Primary Index는 Primary Key에 대해서 생성된 인덱스를 의미
  - 테이블 당 하나의 Primary Index만 존재할 수 있음
- Secondary Index는 Primary Key가 아닌 다른 칼럼에 대해 생성된 인덱스
  - 테이블 당 여러개의 Secondary 인덱스를 생성할 수 있음
 
### Composite Index
- 인덱스로 설정하는 필드의 속성이 중요하다. title, author 이 순서로 인덱스를 설정한다면 title 을 search 하는 경우, index 를 생성한 효과를 볼 수 있음
- author 만으로 search 하는 경우, index 를 생성한 것이 소용이 없어짐.
- SELECT 질의를 어떻게 할 것인가가 인덱스를 어떻게 생성할 것인가에 대해 많은 영향을 끼침

![image](https://github.com/user-attachments/assets/730804ba-690d-407e-9a4f-091b6930f0d0)

![image](https://github.com/user-attachments/assets/161edc0e-bc6a-4e0b-82a1-f5b2fc554217)

![image](https://github.com/user-attachments/assets/252f21ea-f842-4ba3-b87a-b62619ce48f6)

![image](https://github.com/user-attachments/assets/894ac90e-f263-4bb9-81de-d439d5153a70)

# 4. 인덱스의 장단점

### 장점
- 최대 장점은 검색 속도 향상
- 테이블을 만들고 데이터가 쌓이면 record는 순서 없이 저장
- where절을 통해 데이터를 찾아낼 때도 record의 처음부터 끝까지 다 읽어서 검색 조건과 맞는지 비교
- 인덱스를 생성하면 index에는 데이터들이 정렬되어 저장
- 검색 조건에 일치하는 데이터들을 빠르게 찾아낼 수 있음

### 단점
- 추가 저장공간이 필요
  - 테이블의 10%정도의 추가 저장 공간이 필요
- 느린 데이터 변경 작업
  - 검색이 아닌 데이터 변경 작업시 성능이 나빠질 수 있음
  - B+tree구조의 인덱스는 데이터가 추가 삭제 될 때 마다 tree의 구조가 변경될 수 있음
  - 인덱스의 재구성이 필요하기 때문에 추가적인 자원 소모
- Index 생성시, .mdb 파일 크기가 증가
- 한 페이지를 동시에 수정할 수 있는 병행성이 줄어듬
- 인덱스 된 Field에서 Data를 업데이트하거나, Record를 추가 또는 삭제시 성능이 떨어짐
- 데이터 변경 작업이 자주 일어나는 경우, Index를 재작성해야 하므로 성능에 영향을 미침

# 5. 인덱스 효과적으로 사용하는법
- SELECT WHERE 절에 자주 사용되는 Column에 대해 인덱스를 생성
- 수정 빈도가 낮을수록 적합
  - Insert/delete 작업 시 데이터에 변화가 생기기에 매번 정렬을 다시 해야함
- 데이터의 중복성이 높은 Column은 인덱스 효과가 별로 없음
  - 선택도가 낮을 수록 유리 (보통 5~ 10%)
  - 예를 들면 성별의 경우 부적절
- 데이터의 양이 많을 수록 인덱스로 인한 성능 향상이 크고, 적을수록 인덱스의 혜택보단 손해가 더 큼
- Join 조건으로 자주 사용되는 컬럼인 경우
- 한 테이블에 인덱스가 너무 많으면 수정 시 소요되는 시간이 너무 길 수 있음(테이블 당 4~5개 권장)

## 선택도과 카디널리티
- 비례관계
- 선택도 = 카디널리티 / 전체 로우 수
- 선택도가 낮은게 좋은것

다만, 선택도를 무조건 적으로 카디널리티 / 전체 로우 수 로 하는것은 위험하다. 만약 100명의 성별을 기준으로 인덱스를 건다고 했을 때, 카디널리티가 2면 선택도는 2%가 된다. 하지만 평균 성별 분포가 50:50 이라고 했을때 선택도는 해당 (검색하고자 하는 특정 값의 row의 수(=특정 값들의 평균 row수)/100)으로 계산해야 옳다. 그렇다면 선택도는 50/100이 되어 50%가 된다.

[참고 링크](https://yurimkoo.github.io/db/2020/03/14/db-index.html)

## Ref
- [tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/Transaction%20Isolation%20Level.md)
- [Interview_Question_for_Beginner](https://github.com/jbee37142/Interview_Question_for_Beginner/tree/main/Database)
- [기출로 대비하는 개발자 전공면접](https://www.inflearn.com/course/%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%A0%84%EA%B3%B5%EB%A9%B4%EC%A0%91-cs-%EC%99%84%EC%A0%84%EC%A0%95%EB%B3%B5/dashboard)
