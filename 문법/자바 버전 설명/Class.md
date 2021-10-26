## 클래스 속성
- **상속**(Inheritance): 클래스 및 인터페이스가 기존의 클래스나 인터페이스로부터 속성과 동작을 가져오는 것이다.  
자바에서는 extends나 implements 키워드를 사용하여 구현한다.
  - extends: 클래스B extends 클래스A, 인터페이스B extends 인터페이스A (기존의 A를 B가 확장)
  - implements: 클래스 implements 인터페이스 (클래스가 인터페이스를 구현)

  자바의 클래스는 단일상속만 가능하고, 인터페이스는 다중상속이 가능하다.
  - 단일상속: 수퍼클래스가 하나라는 뜻이다. 서브클래스는 여러 개일 수 있다.
  - 다중상속: 인터페이스는 여러 개 가져와서 사용할 수 있다.
  ```
  public ClassB extends ClassA implements Interface1, Interface2 { ... }
  ```

오버라이딩, 단일 상속
오버 로딩
하이딩
추상 클래스
