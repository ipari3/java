타입 추론 관련 문법으로 제네릭, `Optional` 클래스, `var` 키워드가 있다.

## 제네릭 (generic)
단어 뜻 그대로 모든 변수 타입을 포괄한다.  
클래

## Optional
### null
`null`은 종종 오류의 원인이 되곤 한다. 따라서 Nullability를 다루기 위해 `Optional` 클래스를 도입하였다.
> `null`: null은 의미상 값이 없는 상태 혹은 아무런 객체도 가리키지 않는 리터럴이다.  
> 자바에서 `null`은 null 타입의 리터럴이며, 레퍼런스 타입으로 캐스팅될 수 있다. (기본 타입으로는 캐스팅할 수 없다.)

> **Nullability**: null을 값으로 받을 수 있는지 여부이다. 받을 수 있는 변수는 nullable하다.

### 의도
Optional 타입을 클래스 의도에 맞게 사용하려면 다음을 지키는 것이 좋다.
#### `Optional`과 `null`은 함께 쓰지 않는다.
Optional은 non-null이며, 값을 가지고 있는 상태와 가지고 있지 않은 상태의 두 가지를 값으로 가진다.  
(non-null with value, non-null without value)  
값이 없는 상태는 `Optional.empty()`로 나타낸다.  
  
값이 없는 상태가 null과 의미상 같은데, null을 대체하려는 값이니 당연하다.  
또한 Optional과 null을 같이 사용하지 않아야 함도 당연하다.
#### `Optional`은 매개변수에 사용하지 않는다.
메소드 선언 시, null을 대체하는 non-null without value은 오버로딩으로 대체된다.  
예를 들어, `void f(String a)`와 `void f(String a, String b)`를 `void f(String a, Optional<String> b)`로 합치지 않는다.  
합친다 해도, 사용 시에 두 번째 인자를 생략할 수 없고 Optional.empty()라고 넣어주어야 한다.(이것은 원하는 사용법이 아니다.)  
  
여전히 메소드 선언 시에 null에 대한 예외처리를 해주어야 한다.  
다만 호출 시에 인자로 `null`을 넣으면 에러가 발생하고,  
메소드 선언에서의 파라미터 타입과 동일한 변수에 null을 담아 
#### `Optional`과 `null`은 함께 쓰지 않는다.
Optional은 null이 에러를 유발할 때, `null`을 대체하는 용도로 만들어졌다.  
따라서 `Optional`과 `null`을 함께 쓰는 것은 컨셉에 맞지 않다.
```
import java.util.Optional;
import javax.annotation.Nullable;

// 잘못된 사용 예시
public class BadClass {
    public void method() {
        Optional<String> optional = getOptional();
        if (optional != null) { // Optional 타입 인스턴스와 null을 비교하는 것은 맞지 않다.
            // do something
        }
    }
    
    @Nullable // Optional 타입 클래스에 @Nullable 어노테이션을 다는 것은 옳지 않다.
    public Optional<String> getOptional() {
        return null; // Optional 타입 클래스가 null을 반환하는 것은 옳지 않다.
    }
}

// 옳은 사용 예시
public class GoodClass {
    public void method() {
        Optional<String> optional = getOptional();
        optional.ifPresent(
            value -> {
                // do something
            }
        );
    }
    
    public Optional<String> getOptional() {
        return Optional.empty();
    }
}
```
값이 있는지 여부를 `null`이 아닌 `ifPresent` 메소드로 확인할 수 있고,
값이 없다는 상태값은 `null`이 아닌 `empty` 메소드로 표현한다.

IntStream 등의 기본형 스트림에 대응하는 OptionalInt, OptionalLong, OptionalDoule을 이용한다.
