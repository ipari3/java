## 목적
### 컴파일 에러
타입 오류를 컴파일 타임에 잡을 수 있다.  
컴파일 에러는 문법 에러라고도 하며, 컴파일러가 에러의 정확한 위치를 알려준다.
```
// without generic, run-time error
List list = new ArrayList();
list.add("abc");
Integer i = (Integer)list.get(0); // 런타임 에러: String은 Integer로 캐스팅 불가능.

// with generic, compile-time error
List<String> v = new ArrayList<>();
list.add("abc");
Integer i = (Integer)list.get(0); // 컴파일타임 에러: 이유는 동일한데, 컴파일러가 잡아낼 수 있음.
```
### 타입 제한
```
// without generic
List list = new LinkedList();
list.add(new Integer(1));
Integer i = list.iterator().next(); // 컴파일타임 에러: LinkedList 원소들의 타입이 Integer라고 보장할 수 없음.
Integer i = (Integer) list.iterator().next(); // 캐스팅으로 명시해줘야 함. (캐스팅 불가능하면 런타임 에러)

// with generic
List<Integer> list = new LinkedList<>();
list.add(new Integer(1)); // Integer만 추가 가능
Integer i = list.iterator().next();
```
타입을 명시하지 않은 것을 raw type이라고 하며, 대입할 때 타입을 명시해줘야 한다.  
번거로운 것은 물론이고 런타임 에러가 발생할 수 있으므로 매우 좋지 않다.  
자바에서는 제네릭을 사용하는 것을 권장하며, raw type을 쓸 경우 warning을 표시한다.  
   
다만 제네릭은 타입을 제한하여 해당 타입의 원소만 가질 수 있다.  
단일 타입만 가능한 것은 아니고, 만약 `List<Serializable>`이라면 이에 해당하는 Integer, String 등이 모두 가능하다.
### 포괄적인 타입
타입 변수(type variable)을 이용하여 클래스, 인터페이스, 메소드, 생성자를 만들 수 있다.  
클래스 등이 타입 변수를 포함하면 제네릭하다고 하고, 각각 제네릭 클래스 등으로 부른다.
#### type parameter (class, interface)
제네릭인 클래스나 인터페이스는 타입 변수인가 매개변수처럼 사용되며, 이를 타입 매개변수(type parameter)라고 부른다.
```
public class Box<T> {
    private T data;
    
    public void set(T t) {
        this.t = t;
    }
    
    public T get() {
        return t;
    }
}

public interface List<E> {
    void add(E x);
    Iterator<E> iterator();
}

public class Entry<K, V> {
    private final K key;
    private final V value;
    ...
}
```
타입 매개변수는 어떤 이름으로도 지을 수 있지만, 보통 대문자 하나로 짓는다.
- \<T>: type
- \<E>: element
- \<K>: key  
\<V>: value
#### formal type parameter (method, constructor)
```
public <T> List<T> fromArrayToList(T[] a) {
    return Arrays.stream(a).collect(Collectors.toList());
}

public <E> Entry(E element) {
    this.data = element.toString();
    this.rank = element.getRank();
}
```
