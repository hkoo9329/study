#  JPA

## 어노테이션 설명

### 도메인 메핑 어노테이션

![1570175897131](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcKeCWd%2FbtqyM1Ui7i5%2FBlQoqlIbbnJz9cl4M9LxLK%2Fimg.png)

![1570175951773](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fce8ccJ%2FbtqyOUNCv9n%2FvpefVD1sMRKvb8iF5j6Msk%2Fimg.png)

1. @GeneratedValue(strategy = GenerationType.IDENTITY) : 기본 키가 자동으로 할당되도록 설정하는 어노테이션. 

   기본 키 할당 전략을 선택할 수 있는데, 키 생성을 데이터베이스에 위임하는 IDENTITY 전략을 사용하였다.

2. @Enumerated(EnumType.STRING) : Enum 타입 매핑용 어노테이션. 

   @Enumerated 어노테이션을 이용해 자바 enum형과 데이터베이스 데이터 변환을 지원한다. 실제로 자바 enum형이지만 데이터베이스의 String형으로 변환하여 저장하겠다고 선언한 것이다.

3. @OneToOne(fetch = FetchType.LAZY) : 도메인 Board와 Board가 필드값으로 갖고 있는 User 도메인을 1:1 관계로 설정하는 어노테이션.

   실제로 DB에 저장될 때는 User 객체가 저장되는 것이 아니라 User의 PK인 user_idx 값이 저장된다. fetch는 eager와 lazy 두 종류가 있는데 

   - eager

     처음 Board 도메인을 조회할 때 즉시 관련 User 객체를 함께 조회한다는 뜻

   - lazy

     User 객체를 조회하는 시점이 아닌 객체가 실제로 사용될 때 조회한다는 의미

> #### LocalDAteTime
>
> LocalDAteTime은 자바 8에 새로 추가된 기능이다. 기존에는 Date, Calendar 등을 주로 사용했지만 날짜 연산 기능이 많이 부족했다. 그래서 종전에는 날짜에 대한 연산, 비교 등을 API로 제공하는 JodaDateTime을 많이 사용했다. 그런데 LocalDateTime이 제공된 이후로는 번거롭게 JodaDateTime 의존성을 따로 포함할 필요가 없어졌다.  그 이유는 LocalDateTime이 대부분의 날짜 기능을 제공하기 때문이다.





## CommandLineRunner  

CommandLineRunner는 애플리케이션 구동 후 특정 코드를 실행기키고 싶을 때 직접 구현하는 인터페이스이다. 애플리케이션 구동 시 테스트 데이터를 함께 생성하여 데모 프로젝트를 실행/테스트하고 싶을 때 편리하다. 또한, 여러 CommandLineRunner를 구현하여 같은 애플리케이션 컨텍스트의 빈에 사용할 수 있다.





## 서버 사이트 템플릿

### 서버 사이드 템플릿이란?

미리  정의된 HTML에 데이터를 반영하여 뷰를 만드는 작업을 서버에서 진행하고  클라이언트에 전달하는 방식을 말한다. 흔히 사용하는 JSP, 타임리프 등이 서버 사이드 템플릿 엔진이며 스프링 부트 2.0 버전에서 지원하는 템플릿 엔진은 타임리프, 프리마커, 무스타치, 그루비 템플릿 (Groovy Templates) 등이 있다.

### 타임리프

- @{...}

  타임리프의 기본 링크 표현 구문이다. server-relative URL 방식, 즉 동일 서버 내의 다른 컨텍스트로 연결해주는 방식으로 서버의 루트 경로를 기준으로 구문에서 경로를 탐색하여 href의 URL을 대체한다.

- th:each

  반복 구문으로 ${boardList}에 담긴 리스트를 Board 객체로 순차 처리한다. Board 객체에 담긴 getO메서드를 board.O로 접근할 수 있다.