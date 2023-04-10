# Factory Method Pattern

> 객체를 만드는 부분을 Sub Class에 맡기는 패턴

![img](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\Design Pattern\assets\factory.gif)

### 1. 개요

- 객체 생성을 캡슐화 하는 패턴

- 부모(상위) 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며, 자식(하위) 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴



### 2. 구현 시 고려할 점

- 팩토리 메소드 패턴의 구현 방법은 크게 두 가지가 있다.
  - Creator를 추상 클래스로 정희하고, 팩토리 메소드는 abstract로 선언하는 방법
  - Creator가 구체 클래스이고, 팩토리 메소드의 기본 구현을 제공하는 방법
- 팩토리 메소드의 인자를 통해 다양한 Product를 생성하게 한다.
  - 팩토리 메소드에 잘못된 인자가 들어올 겨웅의 런타임 에러 처리에 대해 고민할 것
  - Enum 등을 사용하는 것도 고려할 필요가 있다.
- 프로그래밍 언어별로 구현 방법에 차이가 있을 수 있다.



### 3. Java 예제

- Product 인터페이스

  ```java
  abstract class Product {
      public abstract void use();
  }
  ```

  ``` java
  class IDCard extends Product {
      private String owner;
  
      public IDCard(String owner) {
          System.out.println(owner + "의 카드를 만듭니다.");
          this.owner = owner;
      }
  
      @Override
      public void use() {
          System.out.println(owner + "의 카드를 사용합니다.");
      }
  
      public String getOwner() {
          return owner;
      }
  }
  ```

- createProduct 메소드가 팩토리 메소드

  ```java
  abstract class Factory {
      public final Product create(String owner) {
          Product p = createProduct(owner);
          registerProduct(p);
          return p;
      }
      protected abstract Product createProduct(String owner);
      protected abstract void registerProduct(Product p);
  }
  ```

  ```java
  class IDCardFactory extends Factory {
      private List<String> owners = new ArrayList<>();
  
      @Override
      protected Product createProduct(String owner) {
          return new IDCard(owner);
      }
  
      @Override
      protected void registerProduct(Product p) {
          owners.add(((IDCard) p).getOwner());
      }
  
      public List<String> getOwners() {
          return owners;
      }
  }
  ```

  