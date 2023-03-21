# Template Method Pattern

#### 1. 목적

- 부모 클래스에서 알고리즘의 골격을 정의
- 부모 클래스의 알고리즘 구조를 변경하지 않고 자식 클래스들이 알고리즘의 특정 단계들을 오버라이드(재정의) 할 수 있도록 하는 행동 디자인 패턴



#### 2. 예시 상황

- 회사 문서를 분석하는 데이터 마이닝 앱을 만드는 상황
  - 사용자들은 앱에 다양한 형식(PDF, DOC, CSV)의 문서들을 제공
  - 앱은 일관된 형식으로 의미있는 데이터를 추출
  - 첫번째 앱 : Doc 파일을 분석
  - 두번째 앱 : CSV 파일을 분석
  - 세번째 앱 : PDF 파일을 분석
- 작성한 세가지 앱의 처리하는 데이터는 다르지만 데이터 처리 및 분석을 위한 코드는 거의 유사
  - 그렇다면 알고리즘 구조를 그대로 두되, 코드 중복을 제거하는 것이 좋지 않을까?



#### 3. 장점

1. 중복 코드를 줄일 수 있다.
2. 자식 클래스의 역할을 줄여 핵심 로직 관리가 용이하다
3. 좀 더 코드를 객체지향적으로 구성할 수 있다.



#### 4. 단점

1. 추상 메소드가 많아지면서 클래스 관리가 복잡해진다.
2. 클래스 간의 관계와 코드가 꼬여버릴 염려가 있다.



#### 5. 구조

![img](https://gmlwjd9405.github.io/images/design-pattern-template-method/template-method-pattern.png)

1. AbstractClass
   - 템플릿 메서드를 정의하는 클래스
   - 하위 클래스에 공통 알고리즘을 정의하고 하위 클래스에서 구현될 기능을 primitive 메서드 또는 hook 메서드로 정의하는 클래스
2. ConcreteClass
   - 물려받은 primitive 메서드 또는 hook 메서드를 구현하는 클래스
   - 상위 클래스에 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클래스에 적합하게 primitive 메서드나 hook 메서드를 오버라이드 하는 클래스



#### 6. 예시 코드

```java
//추상 클래스 선생님
abstract class Teacher{
	
    public void start_class() {
        inside();
        attendance();
        teach();
        outside();
    }
	
    // 공통 메서드
    public void inside() {
        System.out.println("선생님이 강의실로 들어옵니다.");
    }
    
    public void attendance() {
        System.out.println("선생님이 출석을 부릅니다.");
    }
    
    public void outside() {
        System.out.println("선생님이 강의실을 나갑니다.");
    }
    
    // 추상 메서드
    abstract void teach();
}
 
// 국어 선생님
class Korean_Teacher extends Teacher{
    
    @Override
    public void teach() {
        System.out.println("선생님이 국어를 수업합니다.");
    }
}
 
//수학 선생님
class Math_Teacher extends Teacher{

    @Override
    public void teach() {
        System.out.println("선생님이 수학을 수업합니다.");
    }
}

//영어 선생님
class English_Teacher extends Teacher{

    @Override
    public void teach() {
        System.out.println("선생님이 영어를 수업합니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Korean_Teacher kr = new Korean_Teacher(); //국어
        Math_Teacher mt = new Math_Teacher(); //수학
        English_Teacher en = new English_Teacher(); //영어
        
        kr.start_class();
        System.out.println("----------------------------");
        mt.start_class();
        System.out.println("----------------------------");
        en.start_class();
    }
}
```





#### 참조

https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html

https://coding-factory.tistory.com/712

https://refactoring.guru/ko/design-patterns/template-method