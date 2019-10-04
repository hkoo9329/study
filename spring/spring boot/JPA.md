#  JPA

## 어노테이션 설명

### 도메인 메핑 어노테이션

![1570175897131](C:\Users\lee\AppData\Roaming\Typora\typora-user-images\1570175897131.png)

![1570175951773](C:\Users\lee\AppData\Roaming\Typora\typora-user-images\1570175951773.png)

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



