## Object
`java.lang.Object`에 있는 public 클래스로, 모든 클래스의 root이다.  
모든 클래스는 Object를 수퍼클래스로 가지며, 이 클래스의 메소드를 구현한다.

#### Object 메소드
- clone
- equals
- toString
- getClass, hashCode
- notify, notifyAll, wait

## Objects
`java.util.Objects`에 있는 public final 클래스다. (뒤에 s가 붙는 복수형이며, 당연히 Object를 extends한다.)  
정적 유틸 메소드로 구성되어 있으며, null-safe 혹은 null-tolerant 메소드 등을 포함한다.  

#### Objects 메소드
- isNull, nonNull, requireNonNull, requireNonNullElse, requireNonNullElseGet
  - `Objects.isNull()`은 `object == null`과 동일하다.  
  코드의 일관성을 위해서는 `object == null`을 사용하는 것이 더 나을 것이며,  
  isNull은 람다 함수 등에서 `filter(Objects::isNull)`과 같은 방식으로 사용할 때 좋다.
- checkIndex, checkFromToIndex, checkFromIndexSize (range 체크)
- compare, equals, deepEquals (대소 비교, 동치 체크)
- toString (Object의 메소드를 오버로딩한 것으로, null인 경우의 출력을 명시되어 있다.)
  - Object.toString()
  - Objects.toString(Object o)
  - Objects.toString(Object o, String nullDefault)
- hash, hashCode
