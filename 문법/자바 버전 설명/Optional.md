## null과 Optional
옵셔널은 널 반환값을 대체하기 위해 만든, 빈 값을 갖는 자료형이라고 볼 수 있다.
- null: 값이 없는 것. true도 false도 아니다.  
null은 의도하고 사용할 수도 있지만, 의도치 않게 값이 들어가지 않은 경우일 수도 있다.  
의도치 않은 null은 다양한 오류를 만들어왔고, null은 기피 대상이 되었다.  
- Optional: null을 대체할 수 있는 클래스이다.
  - 기존: null과 non-null (값이 없음, 값이 있음)
  - Optional: non-null without value와 non-null with value  
  non-null로만 나타낼 수 있다. null이 non-null without value로 대체된다.  
#### 메소드 반환값
그렇다고 Optional이 모든 null을 대체하려고 만들어진 것은 아니며, 오히려 사용을 피하는 것이 좋다.  
Optional이 비싸기 때문이다.  
만들어진 목적은 메소드 호출의 체인이 부드럽게 이어지도록 하는 데 있다.  
즉, 람다식의 반환값에서 null을 대체하는 것이 목적이며, 따라서 반환값으로만 사용하는 것이 좋다.
#### Optional\<T>
옵셔널은 다이아몬드 연산자에 타입을 지정하여 사용한다.  
제네릭도 가능하며, 기본형은 각각 `OptionalInt`, `OptionalLong`, `OptionalDouble`이 제공된다.

## Present or Empty
`isPresent`와 `isEmpty`는 옵셔널 인스턴스가 값을 가지는지 여부를 반환한다.
```
Optional<String> opt = Optional.of("abc");
assertTrue(opt.isPresent());
assertFalse(opt.isEmpty());

opt = Optional.ofNullable(null);
assertTrue(opt.isEmpty());
```
#### ifPresent
람다식에서 옵셔널 인스턴스의 메소드로 사용할 수 있다.
```
opt.ifPresent(x -> f(x));
opt.ifPresent(this::f); // 메소드 하나를 사용하므로 메소드 레퍼런스를 이용할 수 있다.

opt.ifPresent(str -> System.out.println(str.length())); // 메소드가 둘이므로 메소드 레퍼런스를 사용할 수 없다.
```
`ifPresentOrElse`는 present가 아닌 경우의 로직을 두 번째 인자로 받는다.

## 옵셔널 생성
#### 빈 옵셔널
빈 옵셔널은 null을 대체한다.
```
Optional<String> opt = Optional.empty();
```
#### 기본형(Primitive)
기본형은 int, long, double에 해당하는 `OptionalInt`, `OptionalLong`, `OptionalDouble`이 제공된다.
```
OptionalInt opt = OptionalInt.of(0);
OptionalInt stream = IntStream.of(1, 2, 3);
```
기본형은 Optional<Integer> 등으로 사용하지 않고 OptionalInt 등을 사용하는 것이 좋다.
#### 래퍼 타입
- `of`: non-null 값을 받는다. 인자가 null이 아님이 보장될 때 사용한다.
- `ofNullable`: null 값을 받으면 빈 옵셔널 `Optional.empty()`를 반환한다.
```
Optional<String> opt1 = Optional.of(str); // str이 null이면 예외를 던짐
Optional<String> opt2 = Optional.ofNullable(str); // str이 null이면 빈 옵셔널 반환
```
- `or`:
ofNullable이 null 값을 받으면 빈 옵셔널 대신 반환할 옵셔널을 지정할 수 있다.  
null을 받을 때의 값이므로, 당연히 of와는 사용하지 않는다.  
정확히는 옵셔널이 present가 아닐 때 반환할 옵셔널을 지정하는 것이다.
```
opt2.or(Optional.of("def"));
```

## 옵셔널 값
- `get`: 옵셔널이 present면 값을 반환한다. empty면 예외를 던진다.
- `hashCode`: 옵셔널의 값의 해시코드를 반환한다. empty면 0을 반환한다.
```
opt.get(); // empty면 예외 발생
opt.hashCode(): empty면 0 반환
```
다음 메소드들은 옵셔널이 present가 아닐 때 반환할 값을 지정한다. present이면 옵셔널의 값을 반환한다.  
- `orElse`: 인자로 값을 받는다. 그런데 isPresent일 때도 디폴트값이 생성되어 성능이 좋지 않다. (따라서 권장되지 않는다.)
- `orElseGet`: 인자로 Supplier 함수형 인터페이스를 받는다. isPresent일 때는 디폴트값을 생성하지 않는다.  
```
Optional.ofNullable(str).orElse("def"); // 값을 받는다.
Optional.ofNullable(str).orElseGet(() -> "def"); // Supplier 함수형 인터페이스를 받는다.

opt.orElse(getDefault()); // isPresent 여부와 상관없이 getDefault의 내용이 수행된다.
opt.orElseGet(this::getDefault); // isPresent가 아닐 때만 getDefault를 수행한다.

opt.orElse(new String()); // 항상 새로운 문자열을 생성한다.
opt.orElseGet(String::new); // opt가 empty일 때만 문자열을 생성한다.
```
옵셔널이 값을 가지든 가지지 않든 어떠한 값을 받는 일반적인 방법은 `orElseGet`이 좋다.  
isPresent와 get을 이요하는 것은 코드가 길어지고, orElse의 수행방식은 기본적인 사용과 맞지 않다.  
  
빈 옵셔널일 때, 디폴트값이 아닌 예외를 던지게 할 수도 있다.
```
Optional.ofNullable(str).orElseThrow(); // NoSuchElementException을 던진다.
Optional.ofNullable(str).orElseThrow(IllegalArgumentException::new); // 예외 생성자로 직접 지정할 수 있다.
```

## 옵셔널 연산
#### toString
non-empty 문자열 표현을 반환한다.
#### stream
옵셔널의 값만을 갖는 스트림을 반환한다.  
빈 옵셔널이면 빈 스트림을 반환한다.
#### equals
옵셔널은 `==` 같은 identity-sensitive 연산과 사용하지 않는다.  
`==`은 equals로 대체한다.
```
opt1.equals(opt2); // 옵셔널의 값이 아닌 옵셔널을 인자로 넣는다.
```
빈 옵셔널인지 판단할 때도 `opt.isPresent()`를 사용한다. (`opt == Optional.empty()` 처럼 비교하지 않는다.)
> **identity-sensitive 연산**:  reference equality (==), identity hash code, synchronization
#### filter, map, flatMap
[스트림의 연산][1]과 동일하다.

## 주의 사항
#### `null`과 함께 쓰지 않는다.
일반적인 내용에서는 그대로 null을 사용한다.  
옵셔널을 사용할 때는 null을 완전히 대체할 수 있다.  
@Nullable 같은 어노테이션도 함께 쓰지 않는다.
#### 콜렉션과 함께 쓰지 않는다.
콜렉션은 이미 null을 대체할 빈 콜렉션을 갖는다.  
이를 굳이 옵셔널로 감쌀 필요가 없다.  
  
또한 콜렉션의 원소로도 사용하지 않는다.
#### 매개변수에 사용하지 않는다.
옵셔널은 반환값으로만 사용한다고 했다. 또한 매개변수로 사용하기 적합하지도 않다.  
매개변수를 받지 않는 경우는 오버로딩으로 구현하는 것이 옳으며,  
옵셔널 매개변수로 선언하면 안쓰는 매개변수에 굳이 Optional.empty()를 넣어서 호출해야 한다.
```
// 잘못된 선언
public void f(String a, Optional<String> b) { ... }

// 옳은 선언
void f(String a) { ... }
void f(String a, String b) { ... }
```
또한 생성자나 메소드의 반환타입으로도 사용하지 않는다.
#### 필드에 사용하지 않는다.
마찬가지로 필드로 사용하기 적합하지 않다.  
옵셔널은 [`Serializable`][2]을 구현하지 않는다.

[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/Stream.md#%EC%8A%A4%ED%8A%B8%EB%A6%BC-%EC%97%B0%EC%82%B0
[2]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/Serializable%2C%20IO.md#serializable
