# Java Virtual Machine

## 목차
1. Java Virtual Machine
2. JVM의 구성
   - 2.1 Class Loader
   - 2.2 Execution Engine
   - Interpreter
   - JIT
   - Garbage Collector
3. Runtime Data Area
  - 3.1 PC Register
  - 3.2 JVM 스택 영역
  - 3.3 Native method Stack
  - 3.4 Method Area
  - 3.5 Heap
4. Garbage Collection


# 1. Java Virtual Machine
- JVM은 자바 가상 머신의 약자를 따서 줄여 부르는 용어
- JVM은 코드를 실행하고, 해당 코드에 대해 런타임 환경을 제공하는 프로그램에 대한 사양
  - 개발자들은 JVM을 보통 `어떤 기기상에서 실행되고 있는 프로세스, 특히 자바 앱에 대한 리소스를 대표하고 통제하는 서버`를 지칭
- JVM은 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 **실행**하는것
- JAVA와 OS 사이에서 중개자 역할을 수행하여 JAVA가 OS에 구애받지 않고 재사용을 가능하게 해줌
- 가장 중요한 메모리 관리, Garbage Collection을 수행
- 스택 기반의 가상머신
- ARM 아키텍처 같은 하드웨어는 레지스터 기반으로 동작하는데에 비해 JVM은 스택 기반으로 동작

> **가상머신**
>
> 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것

### JVM을 알아야 하는 이유
- 한정된 메모리를 효율적으로 사용하여 최고의 성능을 내기 위해서
- 메모리 효율성을 위해 메모리 구조를 알아야 하기 때문
- 동일한 기능의 프로그램이라도 메모리 관리에 따라 성능이 상이함
- 메모리 관리가 제대로 되지 않으면 속도 저하나 튕김 현상이 발생할 수 있음

### 자바 프로그램 실행 과정
1. 프로그램 실행시 JVM은 OS로 부터 이 프로그램이 필요로 하는 메모리를 할당 받음
   - JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환
3. Class Loader를 통해 class 파일들을 JVM으로 로딩
4. 로딩된 Class파일들은 Execution engine을 통해 해석
5. 해석된 바이트코드는 Runtime Data Areas에 배치되어 실질적인 수행이 이루어짐

- 이러한 실행과정 속 JVM은 필요에 따라 Thread Synchronization과 같은 GC 같은 관리 작업을 수행

![image](https://github.com/user-attachments/assets/436a5aa1-4e29-4aa7-a975-16d4f4f50158)


# 2. JVM의 구성
<img width="695" alt="image" src="https://github.com/user-attachments/assets/868927ff-353b-49b2-8b88-0927c90f5fc8" />

## 2.1 Class Loader(클래스 로더)
- JVM 내로 클래스(.class파일)을 로드하고 링크를 통해 메모리에 배치하는 모듈
- 자바는 실행 중에 클래스를 필요할 때 동적으로 로딩하는 동적 언어임
  - 따라서 클래스는 컴파일 시점이 아니라, 런타임에 실제로 메모리에 탑재
- 이때 필요한 .class 파일들은 보통 jar 파일에 묶여 있고, 클래스 로더는 이 jar 안의 클래스들을 JVM 위로 로드하여 사용하게 만듦
- 더 이상 사용하지 않는 클래스는 JVM이 GC(가비지 컬렉션) 등을 통해 메모리에서 제거
- 즉, 어떤 클래스를 처음 참조하는 시점에 클래스 로더가 해당 클래스를 찾아서 로드하고 링크
  - 이러한 역할을 **클래스 로더(ClassLoader)**가 수행

## 2.2 Execution Engine (실행 엔진)
- 클래스를 실행시키는 역할
- 클래스 로더가 JVM내의 런타임 데이터 영역에 바이트 코드를 배치시키고, 이것은 실행 엔진에 의해 실행
  - 자바 바이트코드는 기계가 바로 수행할 수 있는 언어 보다는 사람이 보기 편한 형태로 기술된 것
- 실행 엔진은 이와 같은 바이트 코드를 실제로 JVM내에서 기계가 실행할 수 있는 형태로 변경
- 이때 두가지 방식이 있음

### 2.3 Interpreter (인터프리터)
- 실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행
  - 하지만 이 방식은 인터프리터 언어의 단점을 그대로 가지고 있음
  - 한 줄씩 수행되어 느림

### 2.3 JIT (Just-In-Time)
- 인터프리터 방식의 단점을 보완하기 위해 도입된 JIT 컴파일러
- 인터프리터 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경
- 이후에 더이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식
- 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일 된 코드는 빠르게 수행
- JIT가 컴파일 하는 과정은 바이트 코드를 인터프리팅 하는 것보다 훨씬 오래걸림
  - 한번만 실행되는 코드라면 컴파일 하지 않고 인터프리팅 하는것이 유리
  - JIT 컴파일러를 이용하는 JVM들은 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크
  - 일정 정도를 넘을때만 컴파일 수행

## 2.4 Garbage collector
GC를 수행하는 모듈(쓰레드)

# 3. Runtime Data Area
- 프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간
![image](https://github.com/user-attachments/assets/601c8269-81b8-436e-9068-ab1328d77474)

## 3.1 PC Register
- 스레드가 시작될 때 생성되며 생성될 때 마다 생성되는 공간으로 스레드별 하나씩 존재
- 스레드가 어떤 부분을 어떤 명령으로 실행해야할 지에 대한 기록을 하는 부분
- 현재 수행 중인 JVM 명령의 주소를 가짐

## 3.2 JVM 스택 영역
- 프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역
- 각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장
- 메소드 호출 시마다 각각의 스택 프레임(그 메서드 만을 위한 공간)이 생성
  - 메서드 안에서 사용되는 값들(local variable) 저장
  - 호출된 메소드의 매개변수, 지역변수, 리턴값, 연산 시 일어나는 값들을 임시 저장
- 메서드 수행이 끝나면 프레임 별로 삭제

## 3.3 Native method Stack
- 자바 프로그램이 컴파일 되어 생성되는 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
- 자바가 아닌 다른 언어로 작성된 코드를 위한 공간
- Java Native Interface를 통해 바이트 코드로 전환하여 저장
- 일반 프로그램처럼 커널이 스택을 잡아 프로그램을 독자적으로 실행시키는 형역
- 이 부분을 통해 C code를 실행시켜 커널에 접근 가능

## 3.4 Method Area (=Class Area =Static Area)
- 클래스 정보를 처음 메모리 공간에 올릴 때 초기화 되는 대상을 저장하기 위한 메모리 공간
- 올라가게 되는 메서드의 바이트 코드는 프로그램의 흐름을 구성하는 바이트 코드
- 자바 프로그램은 main 메서드 호출에서 부터 계속된 메서드의 호출로 흐름을 이어가기 때문임
- 대부분 인스턴스의 생성도 메서드 내에서 명령하고 호출
- 컴파일 된 바이트코드의 대부분이 메서드 바이트 코드이기 때문에 거의 모든 바이트코드가 올라간다고 볼 수 있음
- 이 공간에는 Runtime Constant Pool이라는 별도의 관리 영역 존재
  - 상수 자료형을 저장하여 참조하고 중복을 막는 역할 수행

#### 올라가는 정보의 종류
1. Field Information
   - 멤버 변수의 이름, 데이터 타입, 접근 제어자에 대한 정보 
2. Method Information
   - 메소드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보
3. Type Information
   - class인지 interface인지 여부 저장, Type 속성, 전체 이름, super class 전체 이름(Interface거나 object인 경우 제외)

> - Method Area는 클래스 데이터를 위한 공간이라면 Heap영역이 객체를 위한 공간
> - Heap과 마찬가지로 GC 관리대상에 포함

## 3.5 Heap (힙 영역)
- 객체를 저장하는 가상 메모리 공간
- new 연산자로 생성된 객체와 배열을 저장
- class area 영역에 올라온 클래스들만 객체로 생성 가능
- 힙은 세 부분으로 나눌 수 있음

![image](https://github.com/user-attachments/assets/d5a9ed05-1f27-4eb9-873b-c164a2485761)


### Permanent Generation
- 생성된 객체들 정보의 주소값이 저장
- Class loader에 의해 load는 Class, Method 등에 대한 Meta 정보가 저장되는 영역, JVM에 의해 사용
- Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용
- 내부적으로 Reflection 기능을 자주 사용하는 스프링 프레임워크를 이용할 경우 이 영역에 대한 고려가 필요

> #### Reflection?
>
> - 자바에서 클래스, 메서드, 필드 등의 정보를 실행 중(Runtime)에 동적으로 조회하거나 조작하는 기능
> - 클래스를 코드에서 직접 작성하지 않고, 문자열로 불러와서 사용하는 방식
> - 런타임에 클래스 정보를 읽고 사용할 수 있음
> - new 없이 객체 생성, 메서드 호출, 필드 값 수정 가능
> - 프레임워크(예: 스프링)나 ORM(JPA)에서 아주 자주 사용됨
>
> ```Class<?> clazz = Class.forName("java.lang.String");
> Method[] methods = clazz.getDeclaredMethods();
> for (Method method : methods) {
>     System.out.println(method.getName());
> }
> ```
> - 코드를 보면 Class.forName("java.lang.String")은 String 클래스에 대한 Class 객체를 가지고 옴
> - getDeclaredMethods() 메서드를 사용해서 가져온 클래스에 정의된 모든 메서드의 정보를 얻어내는 것
> - 이렇게 리플렉션을 활용하면, 런타임(Runtime)에 동적으로 클래스의 정보를 얻어내고, 이를 바탕으로 다양한 작업을 할 수 있음
> - 참고 : [자바 리플렉션이란?](https://curiousjinan.tistory.com/entry/java-reflection-explain)

> #### Meta 정보란?
>
> - 클래스, 메서드, 필드 등에 대한 정보(정보에 대한 정보)를 의미
> - 어떤 클래스가 있을때 아래와 같은것들을 메타 정보라고 함
>   - 클래스 이름
>   - 필드 목록
>   - 메서드 목록
>   - 접근 제어자(public/private)
>   - 리턴 타입, 매개변수 타입
>   - 어노테이션 정보

### New/Young 영역
- Eden: 객체들이 최초로 생성되는 공간
- Survivor 0 / 1 : Eden에서 참조되는 객체들이 저장되는 공간

### Old 영역
- New Area에서 일정 시간 참조되고 있는 살아남은 객체들이 저장되고 있던 공간 Eden이 객체가 가득차면 minor GC가 발생
- Eden 영역에 있는 값들을 Survivor 1 영역에 복사하고 이 영역을 제외한 나머지 영역의 객체를 삭제
- 다음 GC 때는 Survivor 1 -> Survivor 0으로 복사
- 반복
- 일정 횟수 넘게 살아남으면 → Old 영역으로 이동

- 인스턴스는 소멸 방법과 소멸 시점이 지역 변수와는 다르기에 힙이라는 별도 영역에 할당
- 자바 가상 머신은 매우 합리적으로 인스턴스를 소멸시킴
- 더 이상 인스턴스의 존재 이유가 없을때 소멸

# 4. Garbage Collection

![image](https://github.com/user-attachments/assets/14bce957-7b60-445b-b7fd-da0300b13302)

- 자바 이전에는 프로그래머가 모든 프로그램 메모리를 관리했음 하지만, 자바에서는 JVM이 프로그램 메모리를 관리
- JVM은 가비지 컬렉션이라는 프로세스를 통해 메모리를 관리
- 가비지 컬렉션은 자바 프로그램에서 사용되지 않는 메모리를 지속적으로 찾아내서 제거하는 역할

## 4.1 Minor GC
- 새로 생성된 대부분의 객체(Instance)는 Eden영역에 위치
- Eden 영역에서 GC가 한 번 발생한 후 살아남으면 Survivor 영역 중 하나로 이동
- 이 과정을 반복하다가 살아남은 객체는 일정시간 참조되고 있다는 뜻이므로 Old로 이동시킴

## 4.2 Major GC
- Old 영역에 있는 모든 객체들을 검사하여 참조되지 않은 객체들을 한꺼번에 삭제
- 시간이 오래 걸리고 실행 중 프로세스가 정지됨
- 이것을 'stop-the-world'라고함
- Major GC가 발생하면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춤
- GC가 완료 된 후 중단했던 작업을 다시 시작

### 소멸시킬 대상을 정하는 원리 참고
- [링크](https://asfirstalways.tistory.com/159)

## Ref
- [자바 가상머신, JVM은 무엇인가?](https://asfirstalways.tistory.com/158)
- [tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20%EC%9E%90%EB%B0%94%20%EA%B0%80%EC%83%81%20%EB%A8%B8%EC%8B%A0(Java%20Virtual%20Machine).md)
