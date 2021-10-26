### 클래스 속성
- **상속**(Inheritance): 클래스 및 인터페이스가 기존의 클래스나 인터페이스로부터 속성과 동작을 가져오는 것.  
자바에서는 extends나 implements 키워드를 사용하여 구현한다.
  - extends: 클래스B extends 클래스A, 인터페이스B extends 인터페이스A (기존의 A를 B가 확장)
  - implements: 클래스 implements 인터페이스 (클래스가 인터페이스를 구현)

  자바의 클래스는 단일상속만 가능하고, 인터페이스는 다중상속이 가능하다.
  - 단일상속: 수퍼클래스가 하나라는 뜻이다. 서브클래스는 여러 개일 수 있다.
  - 다중상속: 인터페이스는 여러 개 가져와서 사용할 수 있다.
  ```
  public class SuperClass {
      public void method() { ... }
  }
  
  // 인터페이스를 여러 개 상속받는 클래스.
  public Subclass extends SuperClass implements Interface1, Interface2 {
      // 수퍼클래스의 메소드를 따로 구현할 필요가 없다.
  }
  ```
  
- **오버라이딩**(Overriding): 수퍼클래스의 메소드를 서브클래스에서 재정의하여 사용하는 것.  
@Override 어노테이션을 이용하여 구현하며, 메소드의 이름과 매개변수는 동일해야 한다. (리턴 타입은 다를 수 있다.)  
오버라이딩한 수퍼클래스의 메소드를 호출하려면 super 키워드를 사용한다.
```
public class SuperClass {
    public void method() {
        System.out.println("SUPER");
    }
}

public class Subclass extends SuperClass {
    @Override // 어노테이션을 써주어야 하며, 수퍼클래스에 없는 메소드에 붙이면 에러 발생
    public void method() {
        super.method(); // 수퍼클래스의 메소드 사용
        System.out.println("SUB");
    }
}
```
> **오버로딩**(Overloading): 메소드 이름은 같지만 매개변수를 다르게 정의하는 것.

- **하이딩**(Hiding): 수퍼클래스의 **정적 메소드**를 서브클래스에서 재정의하여 사용하는 것.  
정적 메소드는 **static** 키워드로 정의된 메소드다.  
하이딩의 구현은 @Override는 사용하지 않고, 메소드의 내용만 재정의한다.  
`Subclass.method()`와 같이 서브클래스 객체의 메소드 호출 시, 수퍼클래스의 메소드를 가리고 재정의한 내용이 호출된다.  
(정적 메소드는 인스턴스화하여 사용하는 것이 권장되지 않으며, 인스턴스화할 경우 좌변의 변수 타입에 해당하는 정적 메소드를 사용한다.)
```
// 선언
public class SuperClass {
    public static void method() {
        System.out.println("SUPER");
    }
}

public class Subclass extends SuperClass {
    public static void method() {
        System.out.println("SUB");
    }
}

// 호출
SuperClass.method(); // print: SUPER
Subclass.method(); // print: SUB

SuperClass a = new Subclass();
a.method(); // print: SUPER (인스턴스의 정적 메소드를 사용하는 것은 권장되지 않는다.)
```
만약 method가 정적 메소드가 아니었다면, a.method()의 결과는 우변의 메소드이며 SUB을 출력할 것이다.

### 클래스 및 메소드 종류
- **추상**(abstract)
