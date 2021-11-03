## Stream API
Stream 인터페이스는 객체들의 시퀀스에 다양한 메소드를 적용하는 파이프라인을 제공한다.
Stream, IntStream, LongStream, DoubleStream은 java.util.stream 패키지에 들어있다.
```
import java.util.stream.Stream;
```

## 스트림 생성
#### 빈(Empty) 스트림
원소가 없는 스트림으로 사용되는 인스턴스다.
```
Stream<String> stream = Stream.empty();
```
#### 기본형(Primitive)
기본형은 int, long, double에 해당하는 `IntStream`, `LongStream`, `DoubleStream`이 제공된다.
```
IntStream stream1 = IntStream.of(1, 2, 3);
IntStream stream2 = IntStream.range(1, 4); // 파이썬의 range처럼 마지막을 포함하지 않는다. 다만 step을 지정할 수 없다.
IntStream stream3 = IntStream.rangeClosed(1, 3); // 마지막을 포함한다.
IntStream stream4 = "abc".chars(); // char에 대한 스트림은 따로 제공되지 않으며, 문자열의 chars 메소드를 통해 IntStream에 넣는다.
```
#### 배열(Array), 컬렉션(Collection)
```
// 배열
Stream<String> stream = Stream.of("a", "b", "c");

String[] arr = new String[]{"a", "b", "c");
Stream<String> streamFull = Arrays.stream(arr);
Stream<String> streamPart = Arrays.stream(arr, 1, 3);

// 컬렉션
Collection<String> collection = Arrays.asList("a", "b", "c");
Stream<String> stream = collection.stream();
```
#### build, generate, iterate
builder()에 원소들을 add()한 후, build()로 종료한다.
builder는 암시적으로 Stream\<Object> 인스턴스를 생성하므로, builder 앞에 원하는 타입을 다이아몬드 연산자에 입력해주어야 한다.
```
Stream<String> stream = Stream.<String>builder().add("a").add("b").add("c").build();
```

`Supplier<T>`를 이용하여 값을 발생시킨다.  
`limit(n)`으로 발생 횟수를 제한해주어야 한다. 그렇지 않으면 메모리 한계치까지 무한정 발생시킨다.
```
Stream<String> stream = Stream.generate(() -> "a").limit(10);
```

마찬가지로 횟수를 제한해주어야 한다.
```
Stream<Integer> stream = Stream.iterate(2, n -> n + 2).limit(10); 2부터 20까지의 짝수
```

## 스트림 연산
#### skipping
```
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> stream = list.stream().skip(3); // 3개를 스킵하고 4, 5만 담는다.
```
#### filtering
```
Stream<String> stream = list.stream().filter(element -> element.contains("a")); // "a"를 포함하는 원소만 가져온다.
```
#### matching
```
boolean containingAny = list.stream().anyMatch(element -> element.contains("a")); // 원소들 중 하나라도 해당
boolean containingAll = list.stream().allMatch(element -> element.contains("a")); // 모든 원소가 해당
boolean containingNone = list.stream().noneMatch(element -> element.contains("a")); // 모든 원소가 비해당

// 빈 스트림에 대해서는 당연히 항상 같은 결과를 얻는다.
Stream.empty().anyMatch(Objects::nonNull); // false (해당하는 원소가 단 하나도 존재하지 않음)
Stream.empty().allMatch(Objects::nonNull); // true (해당하지 않는 원소가 하나도 존재하지 않음)
Stream.empty().noneMatch(Objects::nonNull); // true(해당하는 원소가 하나도 존재하지 않음)
```
> allMatch는 해당하지 않는 원소가 하나도 없음을 확인하여 여부를 확인한다.
#### mapping
각 원소에 함수를 적용시킨다.
```
List<String> uris = new ArrayList<>();
uris.add("C:\\file.txt");
Stream<Path> stream = uris.stream().map(uri -> Paths.get(uri)); // 각 uri를 path로 변환하여 가져온다.
```
`flatMap`은 `map`과 동일한 방식으로 사용하는데,  
각 원소가 자신의 시퀀스를 가질 때 각 내부 원소들에 대해 적용하여 하나의 스트림으로 만든다.
#### collecting
다음은 리스트로 최종 반환하는 예시다.
```
List<String> result = list.stream().map(element -> element.toUpperCase()).collect(Collectors.toList());
```
#### reduction
```
List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5);
Integer product = integers.stream().reduce(1, (a, b) -> a * b); // 5! = 120
```

## 스트림 실전
#### 재사용 불가능
스트림 인스턴스는 일회용으로, 재사용이 불가능하다.
```
// 잘못된 사용: 스트림 재사용
Stream<String> stream = Stream.of("a", "b", "c").filter(element -> element.contains("a"));
Optional<String> anyElement = stream.findAny();
Optional<String> firstElement = stream.findFirst(); // 위에서 사용한 stream을 다시 사용하려고 해서 에러 발생

// 수정: 리스트 인스턴스로 만들어서 재사용
List<String> elements = Stream.of("a", "b", "c").filter(element -> element.contains("a")).collect(Collectors.toList());
Optional<String> anyElement = elements.stream().findAny();
Optional<String> firstElement = elements.stream().findFirst();
```
> `Optional`은 모든 타입을 대변한다.
> 클래스나 메소드 내용 구현 시의 `<T>` 같은 제네릭은 모든 타입을 받을 수 있고,
> `Optional`은 모든 타입 자리에 들어갈 수 있다.
#### 파이프라인
파이프라인은 각 데이터 처리 단계의 출력이 다음 단계의 입력으로 이어지는 구조를 말한다.  
스트림 파이프라인은 **소스**(source), **중간 연산**(intermediate operation), **최종 연산**(terminal operation)으로 이루어진다.  
이 때, 중간 연산은 여러 개 포함될 수 있으며, 최종 연산은 한 개만 쓸 수 있다.  
중간 연산은 스트림을 반환하고, 최종 연산은 비스트림 결과를 반환한다.
```
long size = list.stream().skip(1).map(element -> element.substring(0, 3)).sorted().count();
```
#### 실행 순서
파이프라인은 왼쪽부터 수행되며, 중간 연산의 순서에 따라 성능이 달라질 수 있다.  
스트림 크기(size)를 줄여주는 중간 연산은 원소별로 적용되는 중간 연산보다 먼저 수행되어야 불필요한 연산을 없앨 수 있다.  
스트림 크기를 줄여주는 중간 연산으로는 `skip`, `filter`, `distinct` 등이 있다.

#### 게으른 발동 (Lazy Invocation)
중간 연산은 게으르다(lazy). 이것은 중간 연산이 최종 연산 실행에 필수일 때만 발동됨을 의미한다.  
최종 연산 없이 스트림이 선언되면, 최종 연산에 사용될 때까지 아무 발동도 하지 않는다.  
또한 다음 예시처럼 불필요한 연산을 줄이기도 한다.
```
List<String> list = Arrays.asList("a", "b", "c");
Optional<String> stream = list.stream().filter(element -> {
    log.info("filter() was called");
    return element.contains("b");
}).map(element -> {
    log.info("map() was called");
    return element.toUpperCase();
}).findFirst();
```
1) 첫 번째 원소에 대해 filter가 발동하지만, 충족하지 못하여 다음 원소로 넘어간다.
2) 두 번째 원소에 대해 filter가 발동하고, map을 거쳐 최종 연산인 findFirst를 수행하고 파이프라인이 종료된다.  
findFirst는 원소가 하나만 있어도 실행이 가능하기 때문이다.
3) 따라서 세 번째 원소에 대해서는 filter도 발동하지 않는다.
각 중간 연산이 원소 개수만큼 발동되는 것이 아니라, 필요한 경우에만 발동하여 성능이 향상된다.
