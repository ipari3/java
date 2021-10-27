## 클래스 속성
#### 상속 (Inheritance)
기존의 클래스나 인터페이스로부터 속성과 동작을 가져오는 것.  
자바에서는 extends나 implements 키워드를 사용하여 구현한다.
- **extends**: 클래스B extends 클래스A, 인터페이스B extends 인터페이스A (기존의 A를 B가 확장)
- **implements**: 클래스 implements 인터페이스 (클래스가 인터페이스를 구현)

자바의 클래스는 단일상속만 가능하고, 인터페이스는 다중상속이 가능하다.
- 단일상속: 수퍼클래스가 하나라는 뜻이다. 서브클래스는 여러 개일 수 있다.
- 다중상속: 인터페이스는 여러 개 가져와서 사용할 수 있다.
```
// 선언
public class SuperClass {
    public void method() { ... }
}
  
// 인터페이스를 여러 개 상속받는 클래스.
public Subclass extends SuperClass implements Interface1, Interface2 {
    // 수퍼클래스의 메소드를 따로 구현할 필요가 없다.
}
```
> 자바는 Main을 포함하여 모든 것을 클래스 형태로 구현한다.
> 여기서는 편의상 이를 무시하고 클래스로 묶지 않았다.
  
서브클래스를 인스턴스화하면, 수퍼클래스부터 호출되고 서브클래스가 호출된다.
```
// 선언
public class SuperClass {
    // 생성자 오버라이딩
    public SuperClass() {
        System.out.println("SUPER");
    }
}

public class Subclass extends SuperClass {
    // 생성자 오버라이딩
    public Subclass() {
        System.out.println("SUB");
    }
}

// 호출
new Sub(); // print: SUPER\n SUB \n
```
> **생성자** (Constructor): 객체의 초기화를 담당하는 서브루틴.
> 생성자 이름은 클래스 이름과 동일해야하며 리턴 타입을 명시하지 않는다. 또한 abstract, static, final, synchronized될 수 없다.
> 즉, \[access modifier] ClassName(...) { ... }의 형태로만 존재한다. 접근제어자도 default이다.
> 생성자를 작성하지 않으면, 매개변수와 내용이 비어있는 디폴트 생성자가 자동으로 생성된다.
> (생성자를 하나라도 작성하면 디폴트 생성자는 생성되지 않는다.)
> 오버로딩이 가능하여 여러 생성자를 가질 수 있다.
  
> **접근제어자** (Access modifier):
> - private: 해당 클래스 내에서만 접근 가능
> - default: 해당 패키지 내에서만 접근 가능 (접근 제어자 설정하지 않은 경우이며, package 접근제한자라고도 한다.)
> - protected: 해당 패키지 및 해당 클래스를 상속받은 외부 패키지의 클래스에서 접근 가능
> - public: 모든 클래스에서 접근 가능
> 
> 최상위 클래스는 public이며, default도 가능하다. (protected와 private는 불가능)
> - public: 외부 패키지로 노출
> - default: 해당 패키지에서만 사용
> 외부에 노출되지 않는 클래스는 만드는 의미가 없으므로 기본적으로 public을 사용한다.
> 만약 파일 하나로 끝나는 프로그램이라면 default로도 충분하다.
> 자바에서는 하나의 파일에 하나의 public 클래스만 작성하고, 클래스의 이름은 파일 이름과 동일하게 하여 관리를 용이하게 한다.
> 
> inner 클래스는 private나 protected 접근제어자를 달 수 있다. (inner 클래스는 클래스 내부의 클래스이다.)
  
> **지시자**
생성된 객체 자신의 주소를 참조하는 레퍼런스.

(예시)
public class ClassName {
    private int memVar;
    
    // 값을 넣을 때는 주소를 이용해야 하므로 this를 사용한다.
    public void setMemVar(int memVar) {
        this.memVar = memVar;
    }
    
    // 값만 볼 때는 주소가 필요없다.
    public int getMemVar() {
        return memVar;
    }
}
#### 오버라이딩 (Overriding)
서브클래스에서 수퍼클래스의 메소드를 재정의하는 것.  
**@Override** 어노테이션을 사용하는 것이 권장되며, 메소드의 이름과 매개변수는 동일해야 한다. (리턴 타입은 다를 수 있다.)  
어노테이션을 이용하면 오타를 방지할 수 있고 가독성도 좋아진다.  
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
> 매개변수까지 바꾸는 것 외에도, 오버라이딩은 상속 과정에서 이루어지는데 오버로딩은 상속이 아니더라도 이루어진다.

#### 하이딩 (Hiding)
서브클래스에서 수퍼클래스의 **정적 메소드**를 재정의하는 것.  
정적 메소드는 **static** 키워드로 정의된 메소드로, @Override를 적용할 수 없다.  
따라서 하이딩은 오버라이딩하지 않고 메소드의 내용만 변경하여 재정의하는 것이다.

서브클래스를 통해 메소드를 호출하면, 수퍼클래스의 메소드를 가리고 재정의한 내용이 호출되므로 하이딩이라고 한다.  
(정적 멤버를 인스턴스를 통해 호출하는 것은 권장되지 않는다.)
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
Subclass.method(); // print: SUB (SuperClass의 내용이 가려짐)

SuperClass a = new Subclass();
a.method(); // print: SUPER (권장되지 않음)
```
마지막 코드는 변수 a로 클래스를 인스턴스화하여 메소드를 호출한 것이다.
- 정적 메소드를 하이딩한 것이며, 변수의 자료형인 좌변의 클래스를 따라 SuperClass의 정적 메소드가 호출되었다.
- 만약 일반 메소드를 오버라이딩한 것이었다면, 초기화값인 우변의 인스턴스에 따라 Subclass의 메소드가 호출되었을 것이다.

## 키워드
#### static
클래스의 정적 멤버는 static 키워드로 선언되며, 인스턴스화하기 전에 존재하고 접근가능하다.  
가능한 정도가 아니라, 정적 멤버는 원칙적으로 *클래스를 통해서 호출*하는 것이 권장된다. (*인스턴스를 통한 정적 멤버의 호출*은 <ins>권장되지 않는다.</ins>)  
메소드의 경우는 정적이면 오버라이딩할 수 없으며, 재정의는 하이딩으로 구현된다.
물론 클래스의 멤버가 아니어도 사용할 수 있다.
> **유틸리티 클래스**: 정적 메소드만 가지는 클래스.
> 변수가 없으며 이를 상태가 없다고 말하며, 기능이 비슷한 메소드를 모아놓는다. (상수를 포함할 수도 있다.)
> 보통 인스턴스화와 상속이 불가능하도록 구현한다.
> ```
> // 상속할 수 없도록 final 키워드를 적용한다.
> public final class UtilityClass {
>     // 인스턴스화할 수 없도록 생성자를 private으로 만든다.
>     private UtilityClass() {
>         throw new UnsupportedOperationException(); // 클래스 내부에서 호출되어도 예외 발생
>     }
>     
>     // 인스턴스화할 수 없으므로 메소드들은 정적이어야 한다.
>     public static void method () { ... }
> }
> ```
> 예외를 던지는 것으로도 상속을 막을 수 있지만, 가독성과 에러 추적을 위해서는 final을 사용하는 것이 좋다.
> 예외는 클래스 내부에서 호출되는 것을 막는 역할로 국한한다.

#### abstract
틀만 만들고 내용은 구현하지 않는 것.
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
