# String
자바에서의 스트링

## 목차
1. 스트링의 특징
2. StringBuffer, String Builder

# 1. String의 특징
- new 연산을 통해 생성된 인스턴스의 메모리 공간은 변하지 않음 (Immutable)
- Garbage Collector로 제거되어야 할 대상
- 문자열 연산시 새로 객체를 만드는 Overhead 발생
- 객체가 불변하므로, Multithread에서 동기화를 신경 쓸 필요 없음 (조회 연산에 큰 장점)
- String 클래스는 문자열 연산이 적고, 조회가 많은 멀티쓰레드 환경에서 좋음

```java
public void func() {
    String str1 = new String("HARIBO");
    String str2 = "HARIBO";

    System.out.println(str1 == str2);
    System.out.println(str1.equals(str2));
}
```
- 위 경우 str1 == str2 비교의 결과는 false, equals 결과는 true이다.
- 두개의 문자열은 값은 같지만 실제로는 다른 객체이기 때문

```
public void func() {
    String str1 = "HARIBO"; // 리터럴(literal) 선언
    String str2 = String.valueOf("HARIBO");
        
    System.out.println(str1 == str2);
    System.out.println(str1.equals(str2));
}
```
- valueof() 함수는 매개변수가 null인지 확인 후 널이 아니면 매개 변수의 toString()을 호출
- `String.toString()`은 this를 반환
- 두 구문 모두 "HARIBO"와 같은 리터럴 선언
- 그 이유는 JVM에서 constant pool을 통해 문자열을 관리하기 때문
- 리터럴로 선언한 문자열이 constant pool에 있으면 해당 객체를 바로 가져옴
- 만약 없다면 새로 객체를 생성한 후 poll에 등록하고 가져옴

# 2. StringBuffer, String Builder

| 분류   | String    | StringBuffer | StringBuilder |
| --- | --- | --- | --- |
| 변경   | Immutable | Mutable                         | Mutable              |
| 동기화 |      불필요     | Synchronized 가능 (Thread-safe) | Synchronized 불가능. |

## 공통점
- new 연산으로 클래스를 한번만 만듬 (Mutable)
- 문자열 연산시 새로 객체를 만들지 않고 크기를 변경시킴
- StringBuffer와 StringBuilder 클래스의 메서드가 동일

## 차이점
- StringBuffer는 Thread-Safe
  - 문자열 연산이 많은 멀티 쓰레드 환경
  - 동기화 연산으로 속도가 상대적으로 느림
- StringBuilder는 Thread-safe하지 않음(불가능)
  - 문자열 연산이 많은 Single-Thread 또는 스레드를 신경 안쓰는 환경에서 빠르게 사용하기 유리

## Ref
- [tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5BJava%5D%20Interned%20String%20in%20JAVA.md)
