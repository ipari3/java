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

- Collection\<E>
- List\<E>
- RandomAccess
