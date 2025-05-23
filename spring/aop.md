# AOP

## 목차
1. AOP란?
2. AOP 적용 방식
3. AOP 용어 정리

# AOP란?
- Aspect Oriented Programming, 관점 지향 프로그래밍
- 어떤 로직을 기준으로 핵심 관점과 부가 관점을 나누고, 관점을 기준으로 모듈화하는 것
- 핵심 관점은 주로 핵심 비즈니스 로직
- 부가 관점은 핵심 로직을 실행하기 위한 데이터베이스 연결, 로깅, 파일 입출력 등

## AOP 목적
- 소스 코드에서 여러 번 반복해서 쓰는 코드(= 흩어진 관심사, Concern)를 Aspect로 모듈화하여 핵심 로직에서 분리 및 재사용
- 개발자가 핵심 로직에 집중할 수 있게 하기 위함
- 주로 부가 기능 모듈화


### 부가 기능 적용시 문제
![image](https://github.com/user-attachments/assets/1b62dbf2-d20a-4ec3-927c-04c1fd26a740)

- 부가 기능을 적용할 때 매우 많은 반복
- 부가 기능이 여러 군데에 퍼져 중복 코드 생성
- 부가 기능 변경시 중복으로 인한 많은 수정 필요
- 부가 기능 적용 대상을 변경시 많은 수정 필요

# Aspect
- 부가기능과 부가기능을 어디에 적용할지 선택하는 기능을 합쳐 하나의 모듈로 만듦
  - ex. `@Aspect`
- 어드바이스(부가 기능)과 포인트컷(적용 대상)을 가지고 있어 개념상 하나의 애스팩트
- 애스팩트를 사용한 프로그래밍 방식을 AOP라고 함
  - AOP는 OOP를 대체하기 위한 것이 아니라 횡단 관심사를 깔끔하게 처리하기 어려운 OOP의 부족한 부분을 보조하는 목적으로 개발

![image](https://github.com/user-attachments/assets/1dfb90e6-5649-4792-9299-d6ff2340d7f2)

- 동일한 기능이 흩어져 있으면 유지보수시 어려움 존재
- 각 클래스 내에 흩어진 관심사를 묶어서 모듈화
- 애플리케이션 전체에 걸쳐 사용되는 기능을 재사용


> ### OOP
>
> - 객체지향 프로그래밍
> - OOP는 현실 세계의 개념을 객체로 모델링하여 클래스와 객체 중심으로 프로그램을 구성하는 방식
> - 주요 특징
>   - 캡슐화: 데이터와 메서드를 하나로 묶고, 외부에서 접근 제한
>   - 상속: 기존 클래스의 속성을 확장
>   - 다형성: 동일한 인터페이스로 서로 다른 구현 가능
>   - 추상화:	불필요한 정보 숨기고 핵심 기능만 표현


### AspectJ 프레임워크
- AOP의 대표적인 구현으로 AspectJ 프레임워크가 있음
- 스프링도 AOP를 지원하지만 대부분 AspectJ의 일부 문법을 차용하고, AspectJ가 제공하는 기능의 일부만 제공

# AOP 적용 방식
- AOP를 사용해서 부기 기능 로직이 실제 로직에 추가되는 방법

##  컴파일 시점
![image](https://github.com/user-attachments/assets/1c39381c-caac-48d4-be1b-2516a9baf9f0)
- `.java` 코드를 컴파일러로 사용해서 `.class`를 만드는 시점에 부가 기능 로직 추가
  - AspectJ 컴파일러를 사용해야 함
- 즉, 컴파일될 때 위빙
- 컴파일 시점에 부가 기능을 적용하려면 AspectJ 컴파일러가 필요하고 복잡하기 때문에 잘 사용 X

> 위빙(Weaving): 원본 로직에 부가 기능 로직이 추가되는 것

## 클래스 로딩 시점
![image](https://github.com/user-attachments/assets/031fec64-84de-4a11-8c1a-1c59fd1f7c59)

- 자바는 실행 시 `.class` 파일을 JVM 내부의 클래스 로더에 보관
  - 이때 중간에 .class 파일을 조작해서 JVM에 저장 (java 바이트코드 조작)
- `Java Instrumentation API` 활용
  - jar 파일로 JVM으로 로딩되는 자바 바이트코드를 변형하는 API
  - 모니터링 툴들이 이 방식을 많이 차용
  - 참고
    - https://blog.bespinglobal.com/post/java-instrumentation-api/
    - https://www.baeldung.com/java-instrumentation
- 로드 타임 위빙은 자바를 실행할 때 특별한 옵션(`java -javaagent`)를 통해 클래스 로더 조작기를 지정, 이 부분이 번거롭고 운영이 어려움

## 런타임 시점(프록시)
![image](https://github.com/user-attachments/assets/5c9b5d4a-7341-4e6c-b03e-bfbab9a6f6d9)

- 런타임 시점은 컴파일이 끝나고 클래스 로더에 이미 올라가고, 자바가 실행 된 다음의 시점
  - 자바 언어가 제공하는 범위 내에서 부가 기능을 적용해야 한다는 것을 의미
  - 프록시, DI, 빈 후처리기 개념을 모두 활용

> ### 스프링 용어
>
> - 스프링 컨테이너 (Application Context): 파라미터로 넘어온 설정 클래스 정보(@Configuration)을 사용해 스프링 빈 등록
> - 스프링 빈(Bean): 스프링 컨테이너에 등록된 객체
> - 객체를 외부에서 생성(설정 클래스 정보)하고, 이를 컨테이너에 등록, 외부에서 생성된 객체를 주입해 사용
>
> 1. 객체를 스프링 컨테이너를 통해 전달
> 2. 빈 후처리기에서 AspectJ 모듈 확인, 이를 통해 알맞는 로직이 추가된 프록시 생성
> 3. 생성된 프록시를 스프링 빈에 등록
>
> Proxy Bean 생성
> - 스프링 IoC: 기존 Bean을 대체하는 Dynamic Proxy Bean를 만들어 등록 시켜줌
> - AbstractAutoProxyCreator를 통해 원본 클래스를 감싸는 Proxy Bean을 생성
>   - `AbstractAutoProxyCreator implements BeanPostProcessor`

- 프록시를 사용하기 떄문에 일부 기능 제약
  - 프록시기 때문에 생성자 같은 기능에 제약이 있고, 메서드를 호출 가능
  - 컴파일, 로드 타임 위빙보다 복잡하지 않음

### 프록시 패턴
![image](https://github.com/user-attachments/assets/6dab7625-5437-464d-b457-847fc0802338)

## 위빙 방법 별 정리
- 컴파일 시점: 실제 대상 코드에 에스팩트를 통한 부가 기능 호출 코드가 포함. AspectJ를 직접 사용해야 함
- 클래스 로딩 시점 : 실제 대상 코드에 애스팩트를 통한 부가 기능 호출 코드가 포함. AspectJ를 직접 사용해야 함
- 런타임 시점: 실제 대상 코드는 그대로 유지. 대신 프록시를 통해 부가 기능이 적용
  - 항상 프록시를 통해야 부가 기능을 사용할 수 있음
  - 스프링 AOP는 이 방식을 사용

## AOP 적용 위치
- 적용 가능 지점(조인 포인트): 생성자, 필드 값 접근, static 메서드 접근, 메서드 실행
- AspectJ를 사용해서 컴파일 시점과 클래스 로딩 시점에 적용하는 AOP는 바이트 코드를 실제로 조작하기 때문에 해당 기능을 모든 지점에 적용 가능
- 프록시 방식을 사용하는 스프링 AOP는 메서드 실행 지점에만 AOP 적용 가능
  - 프록시는 메서드 오버라이딩 개념으로 동작(생성자나 static 메서드, 필드 값 접근에 대해 적용 불가)
- 프록시 방식을 사용하는 스프링 AOP는 스프링 컨테이너가 관리할 수 있는 스프링 빈에만 AOP 적용 가능

# AOP 용어 정리
![image](https://github.com/user-attachments/assets/9548f8a9-1b35-4ee4-923e-6b9332f63ccd)

### 조인 포인트(Join Point)
- 어드바이스가 적용될 수 있는 위치, 메소드 실행, 생성자 호출, 필드 값 접근, static 메서드 접근 같은 프로그램 실행 중 지점
- 조인 포인트는 추상적인 개념. AOP를 적용할 수 있는 모든 지점이라고 생각
- 스프링 AOP는 프록시 방식을 사용하므로 조인 포인트는 항상 메소드 실행 지점으로 제한

### 포인트컷(Pointcut)
- 조인 포인트 중에서 어드바이스가 적용될 위치를 선별하는 기능
- 주로 AspectJ 표현식을 사용해서 지정
- 프록시를 사용하는 스프링 AOP는 메서드 실행 지점만 포인트컷으로 선별 가능

### 타겟(target)
- 어드바이스를 받는 객체
- 포인트컷으로 결정

### 어드바이스(Advice)
- 부가 기능
- 특정 조인포인트에서 Aspect에 의해 취해지는 조치
- Around(주변), Before(전), After(후)와 같은 다양한 종류의 어드바이스 존재

### 애스팩트(Aspect)
- 어드바이스 + 포인트 컷을 모듈화 한 것. @Aspect를 생각
- 여러 어드바이스와 포인트 컷이 함께 존재

### 어드바이저(Advisor)
- 하나의 어드바이스와 하나의 포인트컷으로 구성
- 스프링 AOP에서만 사용되는 특별한 용어

### 위빙(Weaving)
- 포인트컷으로 결정한 타켓의 조인포인트에 어드바이스를 적용하는것
- 위빙을 통해 핵심 기능 코드에 영향을 주지 않고 부가 기능을 추가할 수 있음
- AOP 적용을 위해 애스팩트를 객체에 연결한 상태
  - 컴파일 타임
  - 로드타임
  - 런타임, 스프링 AOP는 런타임, 프록시 방식

### AOP 프록시
- AOP 기능을 구현하기 위해 만든 프록시 객체
- 스프링에서 AOP 프록시는 JDK로 동적 프록시 또는 CGLIB 프록시

## Ref
- https://github.com/incutv/tech-interview/blob/master/contents/spring.md#aop%EB%9E%80
