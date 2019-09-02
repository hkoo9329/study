# Coding Interview



## 목차

- JAVA
  - [Overriding vs Overloading](#Overriding-vs-Overloading)
- Spring
- SQL





## JAVA



### Overriding vs Overloading

- 오버라이딩 (Overriding)

  상위 클래스에 존재하는 메소드를 하위 클래스에서 필요에 맞게 재정의하는 것을 의미한다.

- 오버로딩 (Overloading)

  상위 클래스의 메소드와 이름, 리턴 값은 동일하지만, 매개변수만 다른 메소드를 만드는 것을 의미한다. 다양한 상황에서 메소드가  호출될 수 있도록 ==한다==.

 

### Annotation(어노테이션)

어노테이션이란 본래 주석이란 뜻으로, 인터페이스를 기반으로 한 문법이다. 주석과는 그 역할이 다르지만 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있다. 또 해석되는 시점을 정할 수도 있다.(Retention Policy) 어노테이션에는 크게 세 가지 종류가 존재한다. JDK 에 내장되어 있는 `built-in annotation`과 어노테이션에 대한 정보를 나타내기 위한 어노테이션인 `Meta annotation` 그리고 개발자가 직접 만들어 내는 `Custom Annotation`이 있다. built-in annotation 은 상속받아서 메소드를 오버라이드 할 때 나타나는 @Override 어노테이션이 그 대표적인 예이다. 어노테이션의 동작 대상을 결정하는 Meta-Annotation 에도 여러 가지가 존재한다.