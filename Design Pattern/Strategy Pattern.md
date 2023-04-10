# Strategy Pattern

> 어떤 동작을 하는 로직을 정의하고, 이것들을 하나로 묶어(캡슐화) 관리하는 패턴

- **행위를 클래스로 캡슐화**해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
  - 같은 문제를 해결하는 여러 알고리즘이 클래스별로 캡슐화되어 있고 이들이 필요할 때 교체할 수 있도록 함으로써 동일한 문제를 다른 알고리즘으로 해결 할 수 있게 하는 디자인 패턴
- 즉, **전략을 쉽게 바굴 수 있도록** 해주는 디자인 패턴
  - 전략 : 어떤 목적을 달성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘 등
- 특히 게임 프로그래밍에서 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을 때 스트래티지 패턴은 매우 유용

![img](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\Design Pattern\assets\strategy-pattern.png)

- Strategy
  - 인터페이스나 추상 클래스로 동일한 방식으로 알고리즘을 호출하는 방법을 명시
- ConcreteStrategy
  - 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스
- Context
  - 스트래티지 패턴을 이용하는 역할을 수행
  - 필요에 따라 동적으로 구체적인 전략을 바꿀 수 있도록 setter 메서드('집약 관계')를 제공한다.



### 예시

```
[ 슈팅 게임을 설계하시오 ]
유닛 종류 : 전투기, 헬리콥터
유닛들은 미사일을 발사할 수 있다.
전투기는 직선 미사일을, 헬리콥터는 유도 미사일을 발사한다.
필살기로는 폭탄이 있는데, 전투기에는 있고 헬리콥터에는 없다.
```

Strategy pattern을 적용한 설계는 아래와 같다.

![img](https://camo.githubusercontent.com/130af4a613e76e2b9ef0cb940e5f3505bb02c5377e937c807547526e2157d82c/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c65392e75662e746973746f72792e636f6d253246696d61676525324632353543463634313535394537344143303945464242)

**전투기 예시**

```java
class Fighter extends Unit {
    private ShootAction shootAction;
    private BombAction bombAction;
    
    public Fighter() {
        shootAction = new OneWayMissle();
        bombAction = new SpreadBomb();
    }
}
```

`Fighter.doAttack()`를 호출하면, OneWayMissle의 attack()이 호출





### 정리

- 이처럼 Strategy Pattern을 활용하면 로직을 독립적으로 관리하는 것이 편해진다. 로직에 들어가는 '행동'을 클래스로 선언하고, 인터페이스와 연결하는 방식으로 구성하는 것!