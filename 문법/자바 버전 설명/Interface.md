## 인터페이스 멤버
인터페이스는 **상수, 추상 메소드, 디폴트 메소드, 정적 메소드** 4가지 멤버로 구성될 수 있다.
- 인터페이스의 정체성은 다중상속이 가능한 [추상 클래스][1]이며, 원래는 상수와 추상 메소드로만 구성될 수 있었다.
- 그러다 JAVA SE 8부터 필요에 의해 디폴트 메소드와 정적 메소드가 추가되었다.

멤버들은 암시적으로(implicitly) 다음과 같이 선언된다. (즉, 다음을 생략할 수 있다.)
- 상수: `public static final`
- 추상 메소드: `public abstract`
- 디폴트 메소드: `public`
- 정적 메소드: `public`
> 인터페이스 자체는 암시적으로 default 접근제어자를 가진다.
> 클래스에서는 추상 메소드가 암시적으로 default 접근제어자를 가지며 abstract를 생략하면 안된다.

### Before JAVA SE 8
인터페이스는 원래 상수와 추상 메소드만 선언할 수 있었으며, 추상 클래스의 모습을 가지고 있었다.  
따라서 내용은 다른 클래스(구현 클래스)에서 구현되며, 키워드도 `implements`를 사용한다.  
> 인터페이스가 인터페이스를 상속받을 때는 클래스처럼 `extends`를 사용한다.

#### 상수  
인터페이스의 모든 변수는 공개된 상수로, 인터페이스에서 정한 값을 그대로 사용해야 한다. (자동으로 `public static final`이 붙는다.)  
- `static`: 인터페이스는 스스로 인스턴스화할 수 없으므로 인스턴스가 없어도 할당할 수 있어야 한다.
- `final`: 인터페이스는 틀로써 일정해야 한다.
> 상수만을 가진 인터페이스를 **상수 인터페이스**라고 하며, 이는 <ins>권장되지 않는다.</ins>

#### 추상 메소드
틀만 만들고 내용은 구현해놓지 않는다. (추상 클래스의 추상 메소드와 동일)

### From JAVA SE 8
호환성이나 생산성 등을 위해 디폴트메소드와 정적메소드가 추가되었다.  
각각 `default`와 `static` 키워드를 사용하며, 여기서 default는 default 접근제어자하고는 상관 없다.

#### 디폴트 메소드
디폴트 내용이 구현되어 있으며, [오버라이딩][1]이 가능하다.  
하위 호환성을 위해 도입되었으며, defender method 혹은 virtual extension method라고도 부른다.
> **하위 호환성**(backward compatibility)  
> 보통 이전 제품의 인터페이스를 유지한 채 새로운 기능을 추가하는 식으로 이루어진다.  
> 디폴트 메소드가 없을 때 구현 클래스에서 구현된 메소드는 그대로 쓸 수 있기 때문에 호환성이 유지된다.  
> 개인 프로젝트의 경우 구현 클래스 모두 바꾸면 된다지만, 라이브러리 등은 인터페이스 자체를 변경하여 배포해야 한다.

#### 정적 메소드
기본적으로 인터페이스에서 구현한 메소드 내용 그대로 사용해야 한다.  
구현 클래스에서 재정의할 수 있지만, 오버라이딩은 불가능하고 [하이딩][1]이 이루어진다. (인터페이스의 **하이딩**은 <ins>권장되지 않는다.</ins>)
```
public interface InterfaceName {
    // 상수
    double PI = 3.14; // public static final이 자동으로 붙으며, 변수명은 상수에 맞게 짓는다.
    
    // 추상 메소드
    void show(); // 내용은 구현하지 않으며, public abstract가 자동으로 붙는다.

    // 디폴트 메소드
    default void sleep() { ... } // public이 자동으로 붙는다. default는 접근제어자가 아니라 키워드다.
    
    // 정적 메소드
    static void stop() { ... } // public이 자동으로 붙는다.
}
```
## 함수형 인터페이스
1개의 추상 메소드를 갖는 인터페이스이다. (디폴트나 정적 메소드는 여러 개 가질 수 있다.)  
또한 가독성과 유효성 검사를 위해 `@FunctionalInterface` 어노테이션을 적용한다. (없어도 함수형으로 작동할 수는 있다.)
이 어노테이션은 인터페이스가 여러 개의 추상 메소드를 가지면 에러를 발생시킨다.
```
// 예시 1
// 선언
@FunctionalInterface
public interface InterfaceName {
    // 하나의 추상 메소드
    String method();
}

// 사용
InterfaceName itf = () -> "abc";
String str = itf.method();
System.out.println(str); // print: abc

// 예시 2
// 선언
@FunctionalInterface
public interface InterfaceName {
    void method(String str);
}

// 사용
InterfaceName func = str -> System.out.println(str);
func.method("abc") // print: abc
```
기본적인 함수형 인터페이스가 제공되며, 직접 작성하는 경우는 거의 없다.
제공되는 함수형 인터페이스로는 Predicate, Consumer, Supplier, Function<T,R>, Comparator, Runnable, Callable 등이 있다.

[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/Class.md
