# Coding Interview



# 목차

- JAVA
  - [Overriding vs Overloading](#Overriding-vs-Overloading)
  - [Annotation](#Annotation---어노테이션)
  - [Generic ](#Generic---제네릭)
  - [Access Modifier](#Access-Modifier---접근-제어자)
  - [Garbage Collection](#Garbage-Collection)
- Spring
- SQL





## JAVA

## Overriding vs Overloading

- 오버라이딩 (Overriding)

  상위 클래스에 존재하는 메소드를 하위 클래스에서 필요에 맞게 재정의하는 것을 의미한다.

- 오버로딩 (Overloading)

  상위 클래스의 메소드와 이름, 리턴 값은 동일하지만, 매개변수만 다른 메소드를 만드는 것을 의미한다. 다양한 상황에서 메소드가  호출될 수 있도록 한다.

 

## Annotation - 어노테이션

1. #### 어노테이션이란

   - 어노테이션이란 본래 주석이란 뜻으로, 인터페이스를 기반으로 한 문법

   - @를 이용한 주석, 자바코드에 주석을 달아 특별한 의미를 부여나 기능을 주입

   - 컴파일러가 특정 오류를 억제하도록 지시하는 것과 같이 프로그램 코드의 일부가 아닌 프로그램에 관한 데이터를 제공, 코드에 정보를 추가하는 정형화된 방법.

     

2. #### 어노테이션의 용도

   - @Override 어노테이션처럼 컴파일러를 위한 정보를 제공하기 위한 용도

   - 스프링 프레임워크의 @Controller 처럼 런타임에 리플렉션을 이용해서 특수 기능을 추가하기 위한 용도

     ><span style="color: gray">자바 리플렉션 : 다른 언어에는 존재하지 않는 특별한 기능. 컴파일 시간이 아닌 실행시간에 동적으로 특정 클래스의 정보를 객체를 통해 분석 및 추출해내는 프로그래밍 기법</span>

   - 컴파일 과정에 어노테이션 정보로부터 코드를 생성하기 위한 용도

     

3. #### 기본 어노테이션(JDK에서 제공) 

   - @Override  = 해당 메소드가 부모 클래스에 있는 메소드를 재정의 했다는 것을 명시적으로 선언
   - @Deprecated = 더 이상 사용되지 않는 클래스나 메소드 앞에 추가
   - @SuppressWarnings = 프로그램에는 문제가 없는데 간혹 컴파일러가 경고를 뿜을 때가 있는데, 이를 무시하라고 프로그래머에게 알려줌

>출처 :  https://sjh836.tistory.com/8

4.  어노테이션의 해석 시점

   어노테이션에는 크게 세 가지 종류가 존재

   1. `built-in annotation`

      - JDK에 내장 되어 있음

        

   2. Meta annotation`

      - 어노테이션에 대한 정보를 나타내기 위한 어노테이션

        

   3. `Custom Annotation`

      - 개발자가 직접 만들어 내는 어노테이션

 built-in annotation 은 상속받아서 메소드를 오버라이드 할 때 나타나는 @Override 어노테이션이 그 대표적인 예이다. 어노테이션의 동작 대상을 결정하는 Meta-Annotation 에도 여러 가지가 존재한다.

#### Reference

- http://asfirstalways.tistory.com/309



## Generic - 제네릭

제네릭은 자바에서 안정성을 맡고 있다고 할 수 있다. 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에서 사용하는 것으로,  컴파일 과정에서 타입체크를 해주는 기능이다.

객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여준다. 자연스럽게 코드도 더 간결해진다. 예를 들면, Collection 에 특정 객체만 추가될 수 있도록, 또는 특정한 클래스의 특징을 갖고 있는 경우에만 추가될 수 있도록 하는 것이 제네릭이다. 이로 인한 장점은 Collection 내부에서 들어온 값이 내가 원하는 값인지 별도의 로직처리를 구현할 필요가 없어진다. 또한 api를 설계하는데 있어서 보다 명확한 의사전달이 가능해진다.



## Access Modifier - 접근 제어자

변수 또는 메소드의 접근 범위를 설정해주기 위해서 사용하는 Java의 예약어를 의미하며 총 4가지의 종류가 있다.

- public

  어떤 클래스에서라도 접근이 가능,  다른 패키지에서의 접근도 가능

- protected

  클래스가 정의되어 있는 해당 패키지 내, 그리고 해당 클래스를 ==상속받은 외부 패키지의 클래스==에서 접근이 가능하다.

- default

  클래스가 정의되어 있는 해당 패키지 내에서만 접근이 가능하도록 접근 범위를 제한

- private

  정의된 해당 클래스에서만 접근이 가능하도록 접근 범위를 제한한다.



## Wrapper class

기본 자료형 (Primitive data type)에 대한 클래스 표현을 Wrapper class 라고 한다. `Integer`, `Float`, `Boolean` 등이 Wrapper class의 예 이다.

int를 Integer라는 객체로 감싸서 저장해야 하는 이유가 있을까?

일단 컬렉션에서 제네릭을 사용하기 위해서는 Wrapper class를 사용해줘야 한다. 또한 `null`  값을 반환해야만 하는 경우에는 return type으 Wrapper class로 지정하여 `null`을 반환하도록 할 수 있다. 하지만 이러한 상황을 제외하고 일반적인 상황에서 Wrapper class를 사용해야 하는 이유는 객체지향적인 프로그래밍을 위한 프로그래밍이 아니고서야 없다. 

일단 해당 값을 비교할 때 , Primitive data type인 경우에는 == 로 바로 비교해 줄 수 있다. 하지만 Wrapper class인 경우에는 `.intValue()` 메소드를 통해 해당 Wrapper class 의 값을 가져와 비교해줘야 한다.



### AutoBoxing

JDK 1.5 부터는 `AutoBoxing` 과 `AutoUnBoxing`을 제공한다. 이 기능은 각 Wrapper class 에 상응하는 Primitive data type 일 경우에만 가능하다.

```java
List<Integer> lists = new ArrayList<>();
lists.add(1);
```

우린 Integer라는 Wrapper class 로 설정한 collection 에 데이터를 add 할 때 Integer 객체로 감싸서 넣지 않는다. 자바 내부에서 AutoBoxing해주기 때문이다.







## Garbage Collection

### 가비지 컬렉션 이란?

자바는 가비지 컬렉션을 지원하는 대표적인 언어이다. C언어나 C++에서는 메모리를 동적으로 할당한 (힙 영역에 할당) 후 사용을 완료하면 free나 delete 등의 키워드를 사용하여 해체하여야 한다. C언어 에서는 사용한 메모리를 제대호 해제 하지 않으면 메모리 누수나 여러가지 문제가 발생한다.

==가비지 컬렉션==이란 이러한 메모리 해제 작업을 자동으로 해주는 기능을 말한다. 

개발자가 특정 키워드를 이용하여 해제할 메모리영역을 명시하지 않고, 현재 어플리케이션에서 할당된 메모리 중 사용되지 않는 메모리를 해제하여 다시 사용가능 상태로 만드는 것이다. 이것은 전적으로 JVM이 작업을 한다. 

그렇다고 해서 개발자는 가비지 컬렉션만 믿고 마음껏 메모리를 쓰고 메모리 헤제에 대해 신경쓰지 않아도 되는 것은 아니다. 

그 이유는 가비지 컬렉션이 자바 어플리케이션의 성능에 굉장히 많은 영향을 주는 요소이고 개발자는 이것을 잘 튜닝(옵션)하여 현재 사용하고 있는 어플리케이션에 최적화된 가비지 컬렉션이 수행하도록 만들 책임이 있기 때문이다.

>출처 :https://mllab.tistory.com/329