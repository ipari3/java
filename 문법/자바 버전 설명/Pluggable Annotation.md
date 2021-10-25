## Annotation
- 목적: Meta-Data Facility 제공.
- 기능: [Build 및 Run time][1]에 Code Generation, Validation 등의 일을 한다.
> Metadata: 데이터에 대한 데이터로, 행동 양식이나 특정 속성을 설명한다.  
> 주석과 어노테이션이 대표적인 메타데이터이며, 어노테이션은 주석과 다르게 코드에 영향을 준다.
<br></br>
- 형태: @AnnotationName
  어노테이션은 인터페이스와 비슷한 방식으로 정의되며, 따라서 이름은 upper camel case로 짓는다.
- 예시: @Override
```
public class SuperClass {
    public void introduceMyself() {
        System.out.println("Super Class.");
    }
}

public class SubClass extends SuperClass {
    @Override
    public void introduceMyself() {
        System.out.println("Subclass.");
    }
}
```
@Override는 컴파일러에게 오버라이드 되었음을 알려 validation을 확인하도록 한다.
그 외에 @Deprecated는 호출을 막고, @SuppressWarnings는 워닝을 숨긴다.

## Pluggable Annotation
Pluggable Annotation Processing API를 도입함으로써 Customized Annotation Processor를 작성할 수 있게 되었다.
Customized Annotation Processor는 코드에 plugged-in 될 수 있다.


[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/Build,%20Compile,%20Run.md
