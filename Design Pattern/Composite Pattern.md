# Composite Pattern

> Object의 hierarchies를 표현하고 각각의 object를 독립적으로 동일한 인터페이스를 통해 처리할 수 있게 한다.

![composite pattenr](https://github.com/gyoogle/tech-interview-for-developer/raw/master/resources/composite_pattern_1.PNG)

Leaf 클래스와 Composite 클래스를 같은 interface로 제어하기 위해서 Component abstract 클래스를 생성

### Component 클래스

```java
public class Component {
    public void operation() {
        throw new UnsupportedOperationException();
    }
    public void add(Component component) {
        throw new UnsupportedOperationException();
    }
    public void remove(Component component) {
        throw new UnsupportedOperationException();
    }
    public Component getChild(int i) {
        throw new UnsupportedOperation Exception();
    }
}
```

- Leaf 클래스와 Composite 클래스가 상속하는 Component 클래스로 Leaf 클래스에서 사용하지 않는 메소드 호출 시 exception을 발생하도록 구현

### Leaf 클래스

```java
public class Leaf extends Component {
    String name;
    public Leaf(String name) {
        ...
    }
    
    public void operation() {
        ...something...
    }
}
```

### Composite 클래스

```java
public class Composite extends Component {
    ArrayList components = new ArrayList();
    String name;
    
    public Composite(String name) {
        ...
    }
    
    public void operating() {
        Iterator iter = components.iterator();
        while (iter,hasNext()) {
            Component component = (Component)iter.next();
            component.operation();
        }
    }
    public void add(Component component) {
        components.add(component);
    }
    public void remove(Component component) {
        components.remove(component);
    }
    public Componet getChlid(int i) {
        return (Component)components.get(i);
    }
}
```

