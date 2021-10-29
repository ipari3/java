## Immutable vs. Unmodifiable
#### 예시
**Immutable**은 불변이다. 어떤 방법으로도 값이 변하지 않는다.  
**Unmodifiable**은 변경 불가다. 값이 변하게 할 수 없다. 하지만 값이 변할 수는 있다.  
다음의 `ImmutableList` 클래스와 `Collections.unmodifiableList` 메소드를 비교해보자.
```
import com.google.common.collect.ImmutableList;
import Collections;

ImmutableList<String> list1 = ImmutableList.copyOf(list0);
Collection<String> list2 = Collections.unmodifiableList(list0);
```
- `ImmutableList`: list1은 원소를 추가, 수정, 제거할 수 없다. 아무런 변경이 발생할 수 없다.
- `unmodifiableList`: list2는 원소를 추가, 수정, 제거할 수 없다. 그러나 list0이 변하면 list2도 변한다.  
`list0 = Collections.unmodifiableList(list0);`처럼 원본 자체를 unmodifiable로 만드는 것은 메소드의 의도와 맞지 않다.

#### 객체의 목적
두 경우 모두 read-only라는 공통점이 있다. 그러나 세부 의미에서 차이가 있다.  
`ImmutableList`의 목적은 불변이 보장된 리스트를 얻는 것이고, 이것이 read-only가 되었을 뿐이다.  
반면 `unmodifiableList`는 목적 자체가 read-only다. 서브리스트를 읽되, 원본에 영향을 주지 않을 것을 보장해준다.  
(unmodifiableList 코드의 주석에는 비용이 클 수 있는 리스트 복사를 피하면서 서브리스트 읽는 것이 목적이라고 명시되어 있다.)  
> 얕은 불변성(shallow immutability): unmodifiableList는 컬렉션을 래핑(wrapping)한다.  
> 해당 컬렉션이 변하면 래핑한 컬렉션도 변하는데, 이런 특징을 얕은 불변성이라고 한다.

#### 대략적인 결론
- 의미: 불변의 읽기전용 리스트를 원하면 `ImmutableList`를 쓰고, 원본 보존이 보장된 읽기전용 리스트를 원하면 `unmodifiableList`를 쓴다.
- 범위: `unmodifiableList` 자체는 변경될 가능성이 있으므로 작은 scope에서 사용해야 한다. `ImmutableList`는 이것을 신경쓸 필요 없다.

#### Guava
구아바는 구글에서 만든 오픈소스 자바 라이브러리다.  
패키지 경로를 보면 `com.google`로 되어있음을 알 수 있다.  
구아바는 세계적으로 인기있는 자바 라이브러리이며, 다음의 항목들을 주요 특징으로 갖고 있다.
- Collections, String utils
- I/O utils, Concurrency utils
- Caching, Hashing

그 외에 아파치, 잭슨, 잭스비, 모키토, 어썰트J, 하이버네이트, SLF4J, Log4J2 등의 라이브러리가 널리 사용된다.  
([외부 링크에서 더 보기][1])

## ImmutableCollection
서브클래스로는 `ImmutableList`, `ImmutableMultiset`, `ImmutableSet`가 있다.  
Collection과는 다르게 ImmutableCollection은 타입으로 직접 사용하지 않는 것이 좋다.
> **Multiset**은 파이썬의 Counter처럼 각 항목의 개수까지 저장한다.  
> 다만 파이썬의 카운터는 항목과 개수가 key-value의 관계로 value를 직접 증가시켜서 카운팅을 하지만,  
> 자바의 멀티셋은 항목을 추가하거나 제거하면 자동으로 카운팅을 해준다.

#### ImmutableList 메소드
- copyOf, of, toImmutableList, builder, builderWithExpectedSize
- contains, equals, indexOf, lastIndexOf
- forEach, iterator, listIterator, spliterator
- subList, reverse, sortedCopyOf
> - asList는 Deprecated된다. ImmutableList는 List 인터페이스를 구현한 클래스이며, asList는 this를 반환할 뿐이다.  
> - add, addAll, remove, replaceAll은 물론이고 set과 sort는 모두 Deprecated된다. 불변이어야 하기 때문이다.  
> (reverse는 원본을 거꾸로 만드는 것이 아니라, 그러한 view를 반환하는 것이므로 가능하다.)


[1]: https://towardsdatascience.com/top-10-libraries-every-java-developer-should-know-37dd136dff54
