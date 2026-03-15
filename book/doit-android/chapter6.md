# chapter 6. 뷰를 이용한 화면 구성

- 액티비티는 화면을 출력하는 컴포넌트일 뿐이지 그 자체가 화면은 아님
- 화면에 내용을 표시하려면 view 클래스를 이용해야함. 예를 들어 화면을 문자열에 출력하려면 TextView, 이미지를 출력하려면 ImageView를 이용.
- 액티비티 코드로 그릴수도 있고, xml 레이아웃을 이용할 수 있음

## 뷰 클래스
- 액티비티 화면을 구성할 때 사용하는 클래스는 모두 View의 하위 클래스임. 그래서 화면 구성과 관련한 클래스를 통칭하여 뷰 클래스라고 한다.
  <img width="716" height="247" alt="image" src="https://github.com/user-attachments/assets/9cf662a5-4174-45cd-a666-bf2142a682c3" />
- View: 모든 뷰 클래스의 최상위 클래스. 액티비티는 View의 서브 클래스만 화면에 출력
- ViewGroup: View의 하위 클래스지만 자체 UI는 없어서 화면에 출력해도 아무것도 나오지 않음. 다른 뷰 여러개를 묶어서 제어할 목적으로 사용됨. 일종의 그릇 역할을 하는 클래스로, 일반적으로 컨테이너 기능을 담당한다고 함. 실제로는 ViewGroup의 서브 클래스인 레이아웃 클래스를 사용
- TextView: 특정 UI를 출력할 목적으로 사용하는 클래스로, 문자열을 출력하는 뷰
- 뷰의 계층 구조는 레이아웃 객체를 중첩해서 복잡하게 구성할 수도 있음
- 객체를 계층 구조로 만들어 이용하는 패턴을 컴포지트 패턴 또는 문서 객체 모델이라고 함. 
- xml에서 선언한 객체를 코드에서 사용해야 할 때가 있음. 객체를 식별자를 부여하고 그 식별자로 객체를 얻어와야 함. 이때 사용하는 속성이 id
- id는 꼭 지정해야 하는 속성은 아니며 레이아웃 xml에 선언한 뷰를 구별할 필요가 없을 때는 생략해도 됨
- android:id="@+id/text1" 형태로 추가하며, text1이 아이디가 됨. xml 속성값이 @로 시작하면 R.java 파일을 의미함. 따라서 이 표현식은 R.java 파일에 상수 변수로 추가됨.

#### 뷰 크기 
- layout_width, layout_height로 설정
- 수치 / match_parent / wrap_content 세가지로 지정
  - match_parent는 부모 크기 전체. 상위 계층의 크기
  - wrap_content는 자신의 컨텐츠를 화면에 출력할 수 있는 적절한 크기

#### 뷰의 간격 설정
- margin과 padding이 있음
- margin은 뷰와 뷰 사이의 간격이며 padding은 뷰의 콘텐츠와 테두리 사이의 간격

#### 뷰의 표시 여부 설정
- visibible, invisible, gone. invisible은 자리는 차지하지만 보이지 않으며, gone은 자리조차 차지하지 않고 보이지 않음

## 뷰 바인딩
- 뷰 바인딩은 레이아웃 XML 파일에 선언한 뷰 객체를 코드에서 쉽게 이용하는 방법
- 레이아웃 XML에 등록한 뷰는 findViewById() 함수로 얻어서 사용해야 함
- 하지만 위 과정은 귀찮기 떄문에, gradle에 `viewBinding.isEnalbled = true`로 선언하고 xml파일명에 밑줄을 빼고, Binding을 추가함
  - activity_main.xml -> ActivityMainBinding
  - 여기에 inflate() 함수를 호출하면 바인딩 객체를 얻을 수 있음

