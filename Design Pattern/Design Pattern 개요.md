# Design Pattern 개요

> 소프트웨어 공학의 소프트웨어 디자인에서 특정 문맥에서 공통적으로 발생하는 문제에 대해 재사용 가능한 해결책



#### 1. 디자인 패턴 학습의 목적

1. 개발자들과의 원활한 소통을 위함
2. 더 빠르고 유용한 개발을 할 수 있음



#### 2. 디자인 패턴의 원칙

1. Single Responsibility Principle

   > 하나의 클래스는 하나의 역할만 해야 함

2. Open - Close Principle

   > 확장(상속)에는 열려있고, 수정에는 닫혀 있어야 한다.

3. Liskov Substitution Principle

   > 자식이 부모의 자리에 항상 교체될 수 있어야 함

4. Interface Segregation Principle

   > 인터페이스가 잘 분리되어서, 클래스가 꼭 필요한 인터페이스만 구현하도록 해야함

5. Dependency Inversion Property

   > 상위 모듈이 하위 모듈에 의존하면 안됨
   >
   > 둘 다 추상화에 의존하며, 추상화는 세부 사항에 의존하면 안됨



#### 3. 디자인 패턴 종류

1. 생성 패턴 : 5개

   - 객체의 생성 방식 결정

   - 싱글톤(Singleton)
   - 팩토리 메소드(Factory Method)
   - 추상 팩토리(Abstract Factory)
   - 빌더(Builder)
   - 프로토타입(Prototype)

2. 구조 패턴 : 7개

   - 객체 간의 관계를 조직
   - 어댑터(Adapter)
   - 브릿지(Bridge)
   - 컴포짓(Composite)
   - 데코레이터(Decorator)
   - 퍼사드(Facade)
   - 플라이웨이트(Fly Weight)
   - 프록시(Proxy)

3. 행동 패턴 : 11개
   - 객체의 행위의 조직, 관리, 연합
   - 책임 연쇄(Chain-of-Responsibility)
   - 커맨드(Command)
   - 인터프리터(Interpreter)
   - 이터레이터(Iterator)
   - 중재자(Mediator)
   - 메멘토(Memento)
   - 옵저버(Observer)
   - 상태(State)
   - 전략(Stratege)
   - 템플릿 메소드(Template Method)
   - 비지터(Visitor)