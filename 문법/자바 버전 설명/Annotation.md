## Annotation
- 목적: Meta-Data Facility 제공.
- 기능: [Build 및 Run time][1]에 Code Generation, Validation 등의 기능.
> Metadata: 데이터에 대한 데이터로, 행동 양식이나 특정 속성을 설명한다.  
> 주석과 어노테이션이 대표적인 메타데이터이며, 어노테이션은 코드 동작에는 영향이 없지만 빌드나 실행에 영향을 준다.  
> 주석으로 경고했어도 실수를 하고 알아채지 못할 수 있는데, 어노테이션은 컴파일이 알아채고 그냥 넘어가지 않도록 한다.
- 형태: @AnnotationName  
어노테이션은 인터페이스와 비슷한 방식으로 정의되며, 이름도 똑같이 upper camel case로 짓는다.
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
@Override는 컴파일러에게 오버라이드 여부를 알려 validation 검사를 하도록 한다.  
그 외에 @Deprecated는 호출을 막고, @SuppressWarnings는 워닝을 숨긴다.

## Pluggable Annotation
Custom Annotation Processor를 작성할 수 있으며, 이는 코드에 plugged-in 될 수 있다. (기본 어노테이션은 객체 바로 위에 사용.)
커스텀 어노테이션의 정의는 다음과 같으며, 애초에 @interface 어노테이션이 plugged-in 된 형태이다.  
(예시) 필드와 생성자에 적용하는 어노테이션 (필드는 전역 변수 혹은 멤버 변수라고도 한다.)
```
import java.lang.annotation.ElementType;
import java.lang.annoataion.RetentionPolicy;

// 선언
@Target({ElementType.FIELD, ElementType.CONSTRUCTOR})
@Retention(RetentionPolicy.RUNTIME)
public @interface AnnotationName {
    int elementName1();
    int elementName2() default 1; // default 값을 가진 parameter처럼 쓸 수 있다. 
}

// 사용
public class ClassName {
    @AnnotationName(elementName1 = 0, elementName2 = 0)
    private int fieldName;
    
    @AnnotationName(elementName1 = 0) // elementName2 = 1
    public ClassName() {}
}
```
**@Target**
어노테이션을 적용할 대상을 @Target 어노테이션으로 지정할 수 있으며, 여러 개 지정할 수 있다.  
적용 대상은 java.lang.annotation.ElementType에 Enum으로 선언되어 있으며,  
종류는 ANNOTATION_TYPE, CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE, TYPE_PARAMETER, TYPE_USE가 있다.  
TYPE은 클래스, 인터페이스, 어노테이션 타입, 이넘 선언에 사용할 수 있고, ANNOTATION_TYPE은 어노테이션 타입 선언에만 사용할 수 있다.
<br></br>
**@Retention**
어노테이션 언제까지 유지할 것인지 @Retention 어노테이션으로 지정한다.  
java.lang.annoataion.RetentionPolicy에 Enum으로 선언되어 있으며, 종류는 CLASS(default), RUNTIME, SOURCE가 있다.
- SOURCE: 소스 파일까지만 남아있다. 컴파일러에 의해 버려진다.
- CLASS: 자바 바이트 코드(.class 파일)까지만 남아있다. 런타임에 VM에서 유지되지 않는다.
- RUNTIME: 런타임에 VM에서도 유지된다. 따라서 reflective하게 읽을 수 있다.

(예시) 다양한 커스텀 어노테이션 사용 사례
```
Forecast currentForecast = new @Interned Forecast();
@NonEmpty Forecast []
@Readonly object x;
Object myObject = (@NotNull object) obj;
class MyForecast<T> implements @NonEmpty List<@ReadOnly T>
```
> intern: pool에 있다면 그 값을 사용한다.
> 예를 들어, 문자열을 리터럴 대입으로 선언하면 pool에 들어간다. 그리고 다른 변수에 동일하게 선언하면 pool에 있는 값을 사용한다.
> 그런데 new String()으로 선언하면 pool과 상관없이 새로운 리터럴을 생성한다.
> 여기서 intern 한다는 것은 new로 선언해도 pool에 값이 있는지 확인하여 있으면 사용하라는 것이다.

[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/Build,%20Compile,%20Run.md
