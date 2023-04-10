# Observer pattern

> 상태를 가지고 있는 주체 객체 & 상태의 변경을 알아야 하는 관찰 객체
>
> 옵저버 패턴(observer pattern)은 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버들에게 통지하도록 하는 디자인 패턴

![img](https://camo.githubusercontent.com/c0e914f040bf14fcb446e972ca9ca655b1e07f02176f9b278bd0edbf46c8ab9d/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c65392e75662e746973746f72792e636f6d253246696d61676525324632323134323333423536413444393836313134343941)



- Publisher 인터페이스

  > Observer들을 관리하는 메소드를 가지고 있음
  >
  > 옵저버 등록(add), 제외(delete), 옵저버들에게 정보를 알려줌(notifyObserver)

  ```java
  public interface Publisher { 
      public void add(Observer observer); 
      public void delete(Observer observer); 
      public void notifyObserver(); 
  }
  ```

  

- Observer 인터페이스

  > 정보를 업데이터(update)

  ```java
  public interface Observer {
      public void update(String title, String news);
  }
  ```



- NewsMachine 클래스

  > Publisher를 구현한 클래스로, 정보를 제공해주는 퍼블리셔가 됨

  ```java
  public class NewsMachine implements Publisher {
      private ArrayList<Observer> observers; 
      private String title; 
      private String news; 
  
      public NewsMachine() {
          observers = new ArrayList<>(); 
      }
      
      @Override public void add(Observer observer) {
          observers.add(observer);
      }
      
      @Override public void delete(Observer observer) 	{ 
          int index = observers.indexOf(observer);
          observers.remove(index); 
      }
      
      @Override public void notifyObserver() {
          for(Observer observer : observers) {
             observer.update(title, news); 
          }
      } 
      
      public void setNewsInfo(String title, String news) { 
          this.title = title; 
          this.news = news; 
          notifyObserver(); 
      } 
      
      public String getTitle() { return title; } 		public String getNews() { return news; }
  }
  ```



- AnnualSubscriber, EventSubscriber 클래스

  > Observer를 구현한 클래스들로, notifyObserver()를 호출하면서 알려줄 때마다 Update가 호출됨

  ```java
  public class EventSubscriber implements Observer {
      
      private String newsString;
      private Publisher publisher;
      
      public EventSubscriber(Publisher publisher) {
          this.publisher = publisher;
          publisher.add(this);
      }
      
      @Override
      public void update(String title, String news) {
          newsString = title + " " + news;
          display();
      }
      
      public void withdraw() {
          publisher.delete(this);
      }
      
      public void display() {
          System.out.println("이벤트 유저");
          System.out.println(newsString);
      }
      
  }
  ```

  