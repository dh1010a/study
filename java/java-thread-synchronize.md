# 자바 쓰레드 동기화

- 멀티 쓰레드 환경에서 Critical section 문제를 해결하기 위해서는 Critical section에 대해 쓰레드를 동기화 해야함
- 자바에서는 두 가지 방법으로 쓰레드를 동기화 할 수 있음
  1. synchronized 키워드 이용한 암묵적인 동기화
  2. java.util.concurrent.locks 패키지의 lock 클래스들을 이용한 명시적 동기화

## 목차
1. synchronized 키워드를 이용한 암묵적인 동기화
2. lock 클래스들을 이용한 명시적 동기화

# 1. synchronized
- synchronized는 Critical section 설정하는데 사용되면 두가지 방식이 있습니다.

## 메서드 전체를 Critical section으로 지정
```java
public synchronized void exampleMethod(){
	//...
}
```
- 메서드 전체가 Critical section으로 지정되며, 쓰레드는 해당 메서드가 호출된 시점부터 해당 메서드가 포함된 객체의 lock을 얻어 작업을 수행하고 작업이 완료되면 lock을 반환

## 특정 영역을 Critical section으로 지정
```java
synchronized(객체의 참조변수){
	//...
}
```
- 메서드 내의 코드 일부를 블럭으로 감싸고 Critical section으로 지정하는 방식
- 이때 참조변수는 락을 걸고자 하는 객체를 참조하는 것 이어야 하며, 이 블럭에 진입할 때 lock을 얻고 벗어나면 lock을 반환
- 모든 객체는 lock을 하나씩 가지고 있으며, 해당 객체의 lock을 가지고 있는 쓰레드만 Critical Section의 코드를 수행할 수 있음
- 가능하면 메서드 전체에 락을 거는 것 보다 synchronized 블럭을 활용하여 임계 영역을 최소화 하는 것이 좀 더 효율적

## Wait() & notify()
### 개요
- wait()와 notify()는 Object에 정의되어 있으며, synchronized 블럭 내부에서만 사용할 수 있음
- 동기화된 critical section의 코드를 수행하다가 작업을 더 이상 수행할 수 없다면, 일단 wait()를 호출해 쓰레드 lock를 반납하고 기다리게 함. 그러면 다른 쓰레드가 lock을 얻어서 해당 객체에 대한 작업을 수행할 수 있게 됨
- 작업을 진행할 수 있는 상황이 되면 notify()를 호출해서 작업을 중단했던 쓰레드가 다시 락을 얻어 작업을 진행
- 보다 효율적인 동기화를 가능하게 함

### 작동 원리
- wait()가 호출되면 실행중이던 쓰레드는 해당 객체의 waiting pool에서 notify()를 기다리고, notify()가 호출되면 해당 객체의 waiting pool에 있던 쓰레드 중에서 임의의 스레드 하나만 통지를 받음
- wait()에 매개변수로 대기시간을 설정하면 지정된 시간동안만을 기다림
- 지정된 시간이 지난 후에 자동적으로 notify()가 호출되는 것과 같음

### 문제점
1. Race Condition (경쟁 상태)
- notify()는 그저 waiting pool에서 대기중인 스레드 중에 하나를 임의로 선택해서 통지할 뿐이고, 여러 스레드가 락을 얻기 위해 경쟁
- 이를 위해서 스레드를 구별해서 통지하는 등 동기화를 세밀하게 컨트롤 해야하며, 이 경우는 lock 클래스들을 이용해야함

2. starvation (기아 현상)
- 운이 나쁘면 특정 스레드는 계속 notify()를 받지 못하고 오래 기다리는 기아 현상이 발생할 수 있음
- 이를 방지하려면 notifyAll()을 사용해 모든 스레드에게 통지해야함
  - 다시 락을 얻기 위해 경쟁시킴
- 모든 스레드에게 통지를 해도 결국 얻을 수 있는 스레드는 하나뿐

# 2. lock 클래스들을 이용한 명시적 동기화

## java.util.concurrent.locks 주요 클래스
- Java에서 `java.util.concurrent.locks` 패키지는 synchronized보다 더 유연하고 세밀한 동기화 제어를 가능하게 해주는 도구들을 제공

---

## 주요 클래스/인터페이스

| 클래스/인터페이스 | 설명 |
|-------------------|------|
| `Lock` | 명시적으로 락을 걸고 해제할 수 있는 동기화 도구 (`lock()`, `unlock()`) |
| `ReentrantLock` | `Lock`의 구현체. 재진입 가능, `tryLock()` 및 공정성 설정 가능 |
| `ReadWriteLock` | 읽기-쓰기 분리 락을 제공하는 인터페이스 |
| `ReentrantReadWriteLock` | `ReadWriteLock`의 구현체. 읽기/쓰기 락을 나누어 성능 향상 가능 |
| `Condition` | `wait()` / `notify()`를 대체하는 객체. `Lock`과 함께 사용 |
| `StampedLock` | (Java 8+) 낙관적 락(Optimistic Locking) 지원 |
| `LockSupport` | 낮은 수준의 락 및 스레드 제어 유틸리티 (스레드 parking/unparking 등) |

## ReentrantLock
- 가장 일반적인 lock이며, synchronized 블럭의 wait() & notify() 처럼 await() & signal()을 이용해 특정 조건에서 락을 풀고 나중에 다시 락을 얻어 이후의 작업을 수행할 수 있음
- `lock()` / `unlock()`으로 명시적으로 락 관리
- `tryLock()`으로 락 점유 여부 확인 가능
- `new ReentrantLock(true)` → 공정성(Fairness) 설정 가능

- synchronized 블럭은 자동으로 락이 잠기고 풀리기 때문에 편리하지만, ReentrantLock 클래스를 이용하면 다양한 고급 기능 사용 가능
  - Lock polling 지원
  - 코드가 단일 블록 형태를 넘어서는 경우 사용 가능
  - 타임아웃을 지정할 수 있음
  - Condition을 적용해서 대기 중인 스레드를 선별적으로 깨울 수 있음
  - lock 획득을 위해 waiting pool에 있는 스레드에게 인터럽트를 걸 수 있음

```java
// 두가지의 유형
Lock lock = new ReentrantLock();
Lock lock = newReentrantLock(boolean fair);
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```
- ReentrantLock은 두 개의 생성자를 가지고 있으며, fair를 true로 주면 가장 오래 기다린 쓰레드가 lock을 얻을 수 있도록 공정하게 처리
- 공정하게 처리하려면 어떤 스레드가 가장 오래 기다렸는지 확인하는 과정이 필요하므로 성능은 떨어짐

### Method
```java
void lock() // lock 잠금
void unlock() // lock 해제
boolean isLocked() // lock이 잠겼는지 확인
boolean tryLock() // lock polling
boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException
```
- lock()은 lock을 얻을 때 까지 쓰레드를 블락시키므로 context swtich에 따른 overhead가 발생할 수 있음
- 크리티컬 섹션의 수행시간이 매우 짧을 경우 tryLock()을 통한 Lock polling(SpinLock)을 통해 효율적인 locking이 가능
- trylock()에 시간을 설정하면 지정된 시간 동안 Lock을 얻지 못한 경우 다시 작업을 시도할 것인지 포기할 것인지 결정할 수 있음



## Condition
- synchronized의 wait() & notify()는 쓰레드의 종류를 구분하지 않고 공유 객체의 waiting pool에 같이 몰아 넣어 선별적인 통지가 불가능했지만, ReentrantLock과 Condition을 사용하면 쓰레드의 종류에 따라 구분된 waiting pool에서 따로 기다리도록 하여 선별적인 통지를 가능하게 함

- `Object.wait()` / `notify()` 대신 사용하는 조건 객체
- `Lock`과 함께 사용
- 여러 조건을 나누어 처리할 수 있어 `notifyAll()`의 비효율을 개선

```java
Condition condition = lock.newCondition();

lock.lock();
try {
    while (!조건) {
        condition.await();     // wait() 대체
    }
    // 조건 만족 시 작업
    condition.signal();        // notify() 대체
} finally {
    lock.unlock();
}
```

---

## ReentrantReadWriteLock

- 읽기와 쓰기 락을 분리하여 성능 향상
- 읽기는 여러 스레드 허용, 쓰기는 단독 허용

```java
ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();

rwLock.readLock().lock();
// 읽기 작업
rwLock.readLock().unlock();

rwLock.writeLock().lock();
// 쓰기 작업
rwLock.writeLock().unlock();
```

---

## StampedLock (Java 8+)

- `tryOptimisticRead()`로 낙관적인 읽기 가능
- 충돌 없을 경우 락 없이 읽기 가능 → 성능 향상

```java
StampedLock stampedLock = new StampedLock();
long stamp = stampedLock.tryOptimisticRead();
try {
    // 낙관적으로 읽기
    if (!stampedLock.validate(stamp)) {
        stamp = stampedLock.readLock();
        // 재읽기
    }
} finally {
    stampedLock.unlock(stamp);
}
```

---

## LockSupport

- `park()` / `unpark()`로 스레드 제어 (낮은 수준 유틸)
- ForkJoinPool, AQS 등 내부 구현에 사용됨

---

## 도구별 비교

| 도구 | 특징 | 용도 |
|------|------|------|
| `ReentrantLock` | 명시적 락 제어, 재진입 가능 | `synchronized`보다 세밀한 제어 |
| `Condition` | 다중 조건 분기 가능 | 세분화된 wait/notify |
| `ReadWriteLock` | 읽기-쓰기 분리 | 읽기 위주 시스템에 적합 |
| `StampedLock` | 낙관적 락 지원 | 충돌 적은 고성능 읽기 환경 |
| `LockSupport` | park/unpark | 저수준 락 구현 시 사용 |

## Ref
- https://jhkimmm.tistory.com/36


