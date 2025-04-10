# @Transactional

## 목차
1. @Transactional
2. @Transactional 속성
3. 동작 원리


# 1. @Transactional
- 스프링 프레임워크에서 특정 메서드 또는 클래스에서 수행되는 `트랜잭션`과 관련되어 관리를 위해 사용되는 어노테이션
- 메서드 또는 클래스에 해당 어노테이션을 선언하면 코드 실행중에 트랜잭션에 오류가 발행되면 트랜잭션이 `롤백`되고 변경 사항이 모두 취소

### 이점
1. 자동으로 트랜잭션을 시작하고 커밋 또는 롤백할 수 있음
2. 트랜잭션 경계를 명시적으로 설정할 필요가 없음
3. 예외 발생 시 롤백이 처리됨

# 2. @Transactional 속성
|속성|설명|속성 값|
| --- | --- | --- |
|propagation|	트랜잭션 전파 동작을 지정|	REQUIRED, SUPPORTS, MANDATORY, REQUIRES_NEW, NOT_SUPPORTED, NEVER, NESTED|
|isolation|	트랜잭션 격리 수준을 지정|	DEFAULT, READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE|
|timeout|	트랜잭션 제한 시간을 지정|	초 단위의 정수 값|
|readOnly|	트랜잭션이 읽기 전용인지 지정|	true, false|
|rollbackFor|	롤백을 일으키는 예외를 지정|	예외 클래스|
|noRollbackFor|	롤백을 일으키지 않는 예외를 지정|	예외 클래스|

## Propagation 속성
|propagation 상세 속성 값 	|설명|
| --- | --- |
|REQUIRED	|현재 트랜잭션이 존재하면 그 트랜잭션 내에서 실행하고, 트랜잭션이 없으면 새로운 트랜잭션을 시작합|
|SUPPORTS	|현재 트랜잭션이 존재하면 그 트랜잭션 내에서 실행하고, 트랜잭션이 없으면 트랜잭션 없이 실행|
|MANDATORY	|현재 트랜잭션이 존재하면 그 트랜잭션 내에서 실행하고, 트랜잭션이 없으면 예외를 발생시킴|
|REQUIRES_NEW	|항상 새로운 트랜잭션을 시작하고, 현재 트랜잭션이 존재하면 일시정지시킴|
|NOT_SUPPORTED	|트랜잭션 없이 실행하고, 현재 트랜잭션이 존재하면 일시정지시킴|
|NEVER	|트랜잭션 없이 실행하고, 현재 트랜잭션이 존재하면 예외를 발생시킴|
|NESTED	|현재 트랜잭션이 존재하면 중첩된 트랜잭션으로 실행하고, 트랜잭션이 없으면 REQUIRED와 같이 동작|

## Isolation 속성
|Isolation 속성 값	|설명|
| --- | --- |
|DEFAULT|	기본 격리 수준을 사용함|
|READ_UNCOMMITTED	|커밋되지 않은 데이터에 대한 읽기를 허용(dirty read)|
|READ_COMMITTED	|커밋된 데이터만 읽기를 허용 (Oracle 기본)|
|REPEATABLE_READ	|같은 쿼리를 반복해서 실행할 때 같은 결과를 보장|
|SERIALIZABLE	|동시성 문제를 완전히 막기 위해 모든 트랜잭션을 순차적으로 처리|

# 3. 동작 원리
- @Transactional 어노테이션이 적용된 메서드는 Spring의 AOP 기능을 통해 트랜잭션 관리가 이루어짐
- Spring은 @Transactional이 붙은 메서드를 호출할 때, 해당 메서드를 프록시 객체로 감싼다
- 프록시 객체는 메서드 호출 전후에 트랜잭션을 시작하고 종료하는 역할
- Spring은 JDK 동적 프록시나 CGLIB 프록시를 사용하여 이 작업을 수행

1. 메서드 호출 전: 프록시 객체는 트랜잭션을 시작. 이 때 기존 트랜잭션이 존재하는지 확인하고 전파 속성에 따라 트랜잭션을 새로 시작하거나 기존 트랜잭션에 참여
2. 메서드 실행: 실제 비즈니스 로직이 실행
3. 메서드 호출 후: 프록시 객체는 트랜잭션을 종료. 정상적으로 메서드가 실행되면 커밋하고, 예외가 발생하면 롤백

![image](https://github.com/user-attachments/assets/b01467dc-6cca-4751-82fc-1731f0caf460)

## 주의할 점
1. 메서드가 private로 되어 있으면 @Transactional 어노테이션을 사용할 수 없음
  - 프록시 객체는 타겟 객체/인터페이스를 상속 받아서 구현
  - private로 되어 있으면 자식인 프록시 객체에서 호출할 수 없음
  - 그래서 @Transactional이 붙는 메서드, 클래스는 프록시 객체에서 접근 가능한 레벨로 지정
2. 트랜잭션은 객체 외부에서 처음 진입하는 메서드를 기준으로 동작
  - @Transactional 어노테이션이 붙은 메서드 안에 어떤 메서드를 호출하는데 그 메서드에도 붙어있어도 AOP Proxy는 호출 시점에 target을 가로채기 때문에 처음 진입하는 메서드를 기준으로 동작


## Ref
- https://adjh54.tistory.com/378
- https://jhyonhyon.tistory.com/67
