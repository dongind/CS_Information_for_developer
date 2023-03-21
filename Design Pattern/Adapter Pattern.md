# Adapter Pattern

#### 1. 어댑터 패턴의 정의

- 호환성이 없는 기존 클래스의 인터페이스를 변환하여 사용자가 기대하는 인터페이스 형태로 변환시키는 패턴
- 코드의 재활용성을 증가하고 기존의 코드를 수정하지 않는 장점이 있음



#### 2. 어댑터 패턴 사용 시점

- 외부 구성 요소를 기존 시스템에 재사용하고 싶지만 호환되지 않는 경우
- 애플리케이션이 클라이언트가 기대하는 인터페이스와 호환되지 않는 경우
- 원본 코드를 수정하지 않고 애플리세이션에서 레거시 코드를 재사용하련느 경우



#### 3.  어댑터 패턴 클래스 다이어그램

![img](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\Design Pattern\assets\img.png)

- 객체 지향 어댑터 

  - 어떤 인터페이스를 클라이언트에서 요구하는 형태의 인터페이스에 적응시켜주는 역할

  ![img](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\Design Pattern\assets\256C6A47575EB04330.png)



#### 4. 어댑터 패턴 구조

```java
// Adaptee 는 이미 개발이 완료됐고, 수정하기 곤란한 상황이다.
class Adaptee {
  Response specificRequest() {
    return new Response();
  }
}

// 클라이언트가 사용하려는 인터페이스.
interface Target {
  Response request();
}

// Adapter는 Adaptee를 감싸고 있으며, Target 인터페이스를 구현한다.
class Adapter implements Target {
  private final Adaptee adaptee;

  public Adapter(Adaptee adaptee) {
    this.adaptee = adaptee;
  }

  @Override
  public Response request() {
    return this.adaptee.specificRequest();
  }
}
```



#### 5. 어댑터 패턴 예제

- 오리를 표현하는 코드

  ```java
  interface Duck {
    public void quack();  // 오리는 꽉꽉 소리를 낸다
    public void fly();
  }
  
  class MallardDuck implements Duck {
    @Override
    public void quack() {
      System.out.println("Quack");
    }
    @Override
    public void fly() {
      System.out.println("I'm flying");
    }
  }
  ```

  

- 칠면조를 표현하는 코드(오리와 코드 구조가 다르다는 점을 주목)

  ```java
  interface Turkey {
    public void gobble();   // 칠면조는 골골거리는 소리를 낸다
    public void fly();
  }
  
  class WildTurkey implements Turkey {
    @Override
    public void gobble() {
      System.out.println("Gobble gobble");
    }
    @Override
    public void fly() {
      System.out.println("I'm flying a short distance");
    }
  }
  ```

- 어댑터(Turkey를 Duck처럼 사용할 수 있게 해주는 어댑터 코드)

  ```java
  class TurkeyAdapter implements Duck {
    Turkey turkey;
  
    public TurkeyAdapter(Turkey turkey) {
      this.turkey = turkey;
    }
    @Override
    public void quack() {
      turkey.gobble();
    }
    @Override
    public void fly() {
      // 칠면조는 멀리 날지 못하므로 다섯 번 날아서 오리처럼 긴 거리를 날게 한다
      for (int i = 0; i < 5; i++) {
        turkey.fly();
      }
    }
  }
  ```

- 실행 코드

  ```java
  package AdapterPattern;
  
  public class DuckTest {
  
  	public static void main(String[] args) {
  
  		MallardDuck duck = new MallardDuck();
  		WildTurkey turkey = new WildTurkey();
  		Duck turkeyAdapter = new TurkeyAdapter(turkey);
  
  		System.out.println("The turkey says...");
  		turkey.gobble();
  		turkey.fly();
  
  		System.out.println("The Duck says...");
  		testDuck(duck);
  
  		System.out.println("The TurkeyAdapter says...");
  		testDuck(turkeyAdapter);
  
  	}
  
  	public static void testDuck(Duck duck) {
  
  		duck.quack();
  		duck.fly();
  
  	}
  }
  ```

  





#### 참조

[디자인패턴] [Adapter] 어댑터 패턴 : https://wellsw.tistory.com/240