## 클래스 속성
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
@Override 어노테이션을 적용하는 것이 권장되며, 메소드의 이름과 매개변수는 동일해야 한다. (리턴 타입은 다를 수 있다.)  
(어노테이션을 이용하면 오타를 방지할 수 있고 가독성이 좋다.)  
> **super**: 오버라이딩한 수퍼클래스의 메소드를 호출하려면 super 키워드를 사용한다.
```
public class SuperClass {
    public void method() {
        System.out.println("SUPER");
    }
}

public class Subclass extends SuperClass {
    @Override
    public void method() {
        super.method(); // 수퍼클래스의 메소드 사용
        System.out.println("SUB");
    }
}
```
> **오버로딩**(Overloading): 메소드 이름은 같지만 매개변수를 다르게 정의하는 것.

- **하이딩**(Hiding): 수퍼클래스의 **정적 메소드**를 서브클래스에서 재정의하여 사용하는 것.  
정적 메소드는 **static** 키워드로 정의된 메소드다.  
정적 메소드에는 @Override를 적용할 수 없으며, 메소드의 내용만 재정의하여 하이딩한다.  
서브클래스 객체의 메소드 호출 시, 수퍼클래스의 메소드를 가리고 재정의한 내용이 호출된다.  
(정적 메소드는 객체를 통해 호출되며, 인스턴스를 통해 호출하는 것은 권장되지 않는다.)
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
a.method(); // print: SUPER (권장되지 않음)
```
클래스를 인스턴스화하여 메소드를 호출할 때,  
정적 메소드는 권장되지 않지만 좌변의 클래스 객체를 따르고, 정적 메소드가 아니면 우변의 인스턴스를 따른다.

## 키워드
- **static**(정적): 인스턴스화하기 전에 존재하며, 따라서 인스턴스 없이 호출할 수 있다.  
인스턴스에서 호출할 수는 있지만 권장되지 않는다.  
또한 오버라이딩할 수 없다.
> 유틸리티 클래스: 정적 메소드와 변수(상수)만 가지는 클래스.
> 상태가 없다고 표현하며, 기능이 비슷한 메소드와 상수를 모아놓은 것이다.

- **abstract**(추상): 틀만 만들고 내용은 구현하지 않는 것. 메소드와 클래스 모두에 적용할 수 있다.  
  - 추상 메소드: abstract가 붙은 메소드이며, 내용을 구현하면 안된다. ({} 없이 ();로 선언을 끝낸다.)   
  단순히 내용을 {}로 비워놓을 수도 있지만 가독성과 오타 방지등을 위해 abstract를 이용하는 것이 좋다.
  - 추상 클래스: abstract가 붙은 클래스이며, 추상 메소드를 하나라도 가지면 abstract를 붙여야 한다.  
  추상 메소드를 가지고 있지 않아도 abstract를 적용할 수 있지만, 의미가 없으며 가독성을 위해 권장되지 않는다.
    - 구현 강제: 추상 메소드는 추상 클래스를 상속받는 클래스에서 구현해야만 한다.  
    상속받은 클래스가 추상 클래스면 구현을 미루어도 된다.
    - 인스턴스화 불가능: 추상 클래스는 인스턴스화할 수 없다.  
    그러나 이것만이 목적이라면 abstract 보다는 protected를 이용하는 것이 낫다.
```
public abstract class SuperClass {
    public abstract void method();
    public void method1() {} // 권장되지 않음.
}

public abstract class MidClass extends SuperClass {
    // 추상 클래스는 추상 메소드를 구현하지 않고 남겨놓을 수 있다.
}

public class Subclass extends MidClass {
    @Override
    public void method() { ... }
}
```

- **final**: 최종 결과라는 의미로, 수정이 불가하다.  
  - final 클래스: 상속 금지. 상속 시 에러 발생.
  - final 메소드: 재정의 및 오버라이딩 금지. @Override를 사용하지 않고 내용을 변경해도 에러 발생.
```
public class SuperClass {
    public final void method() { ... }
}

public class Subclass extends SuperClass {
    // 재정의 불가능
    // public final void method() { ... }
}
```
