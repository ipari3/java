## 선언
```
import java.util.ArrayList;

ArrayList<Integer> numbers = new ArrayList<>();
ArrayList<Integer> numbers = new ArrayList<>();
ArrayList<Integer> numbers = new ArrayList<>();
```

## 인터페이스
Serializable, Cloneable, Iterable, Collection, List, RandomAccess
#### Serializable
semantic. (메소드나 필드 없음)
#### Cloneable
- `public Object clone()`: 얕은 복사. Object 타입(타입캐스트 필요)
```
ArrayList<Integer> clone = (ArrayList<Integer>) orig.clone();
```
#### Iterable\<E>
- `default void forEach(Consumer<? super E> action)`
```
// foreach 구문처럼 작동한다.
for (E e : this)
    action.accept(e);

// 람다식 사용
numbers.forEach(elt -> System.out.println(elt));
```
- `Iterator<E> iterator()`: Iterator 타입
  - `default void forEachRemaining(Consumer<? super E> action)`: 람다식 형태
  - `boolean hasNext()`
  - `E next()`
  - `default void remove()`
```
// 생성
Iterator it<Integer> = numbers.iterator();

// 순회
while(it.hasNext()) {
    int(it.next());
}

```
- Collection\<E>
- List\<E>
- RandomAccess
