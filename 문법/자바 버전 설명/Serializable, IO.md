## Serializable
`Serializable` 인터페이스를 구현한 클래스의 객체는 직렬화 및 역직렬화가 가능하다.  
serializable 클래스는 다음의 조건을 만족해야 한다.
- `java.io.Serializable` 인터페이스를 `implements`한다.
- 클래스의 필드는 serialzable해야 한다.  
serialzable하지 않은 필드는 `transient`여야 한다.
```
public class Employee implements java.io.Serializable {
    public String name;
    public transient int SSN;
}
```
> **transient**: 트랜지언트 데이터는 직렬화에서 제외된다.  
> 주로 사회보장번호나 비밀번호 등의 보안 정보가 이에 해당한다.

## 직렬화 (Serialization)
직렬화는 다차원의 데이터를 일차원으로 연결(직렬)하는 것이며,  
자료를 파일로 저장하거나 네트워크로 전송하기 위해 수행한다.  
([외부에서 더 보기][6])  
  
자바 객체에 대입해보면, 객체의 다양한 상태(필드)를 바이트스트림으로 변환하는 것이며,  
바이트스트림은 데이터베이스로 저장되거나 네트워크로 전송될 수 있다.  
**역직렬화**(Deseriealization)는 그 반대 과정이다.  
넓은 의미의 직렬화는 역직렬화를 포함하여, 객체를 바이트로 변환하였다가 다시 사용가능한 객체로 복구하는 것을 말한다.
#### serialize
직렬화는 쓰기(write)위한 스트림인 OutputStream 관련 클래스들을 이용한다.  
직렬화 결과를 파일로 저장할 때, 표준 확장자는 **.ser**이다. (txt 등을 사용해도 상관없다.)
```
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;

try (
    FileOutputStream fileOut = new FileOutputStream("file.ser");
    ObjectOutputStream objOut = new ObjectOutputStream(fileOut);
) {
    objOut.writeObject(list);
} catch (IOException e) {
    return null;
}
```
> [**try-with-resources**][1]를 사용하여 코드를 줄일 수 있다.
#### deserialize
역직렬화는 읽기(read)위한 스트림인 InputStream 관련 클래스들을 이용한다.  
```
import java.io.FileInputStream;
import java.io.ObjectInputStream;

Employee employee = null; // scope을 고려하면 try 외부에서 선언하는 것이 옳다.
try (
    FileInputStream fileIn = new FileInputStream("/tmp/file.ser");
    ObjectInputStream objIn = new ObjectInputStream(fileIn);
) {
    employee = (Employee) objIn.readObject();
} catch
    return null;
}
```
## IO Stream
#### Object-Stream
- `ObjectOutputStream`: 기본형 데이터와 객체 그래프를 OutputStream에 쓴다. 이 결과는 ObjectInputStream을 통해 읽을 수 있다.
- `ObjectInputStream`: ObjectOutputStream으로 쓴 기본형 데이터와 객체를 역직렬화한다.  
역직렬화는 리소스 관리나 직렬화한 데이터에 대한 보안 등을 신경써야 한다.  
(외부에서 더 보기: [시큐어 코딩][2], [직렬화 필터링][3])  
  
각각 OutputStream, 읽을 InputStream을 인자로 받는다.
그리고 각각 write 관련, read 관련 메소드를 갖는다.
#### ByteArray-Stream
- `ByteArrayOutputStream`: 바이트 어레이를 OutputStream에 쓴다.  
데이터가 쓰이면 버퍼가 자동으로 증가한다.  
- `ByteArrayInputStream`: 스트림에서 바이트 어레이를 읽는다.  
내부 버퍼를 포함하며, 내부 카운터가 다음 바이트를 추적한다.
두 클래스는 `close()`하지 않는다. (아무 효과가 없다.)  
이 클래스들의 메소드는 해당 스트림이 닫혀도 호출될 수 있다.  
  
ByteArrayOutputStream는 인자를 받지 않거나, int로 size를 받는다.  
ByteArrayInputStream는 byte\[] 버퍼를 받으며, int offset과 int length를 추가로 받을 수도 있다.
각각 write 관련, read 관련 메소드를 갖는다.  
또한 ByteArrayOutputStream는 `toByteArray()`와 `toString()` 메소드로 데이터를 얻을 수 있다.  
  
위의 예시 코드에서는 파일로 저장했다가 불러왔는데,  
파일로 저장할 필요없이 바이트어레이로 해결하려면 파일 관련 클래스 대신 이 클래스들로 대체하면 된다.  
이때 ByteArrayOutputStream는 인자로 받을 byte\[] 변수가 필요하다.
#### Filter-Stream
다른 조작을 위한 스트림을 포함한다.  
서브클래스로 Buffered-Stream, Data-Stream 등이 있다.  
(외부에서 더 보기: [oracle docs][4], [기타 설명][5])
#### File-Stream
위의 두 클래스와 방향이 반대다. (Object, ByteArray, Filter <-> In/Out-putStream <-> File)
- `FileOutputStream`: raw 바이트 스트림을 File 혹은 FileDescriptor로 쓸 때 사용한다.
- `FileInputStream`: 파일에서 바이트 스트림을 읽어온다.

둘 다 File, FileDescriptor, String 중 한 타입의 인수를 받는다. (String 인자는 경로 및 파일명)  
각각 write 관련, read 관련 메소드를 갖는다.  

> **raw bytes**: gap이나 마커로 구분되지 않고 나열된 바이트들이다. 미가공 바이트라고 번역하기도 한다.
이미지 데이터 등이 raw 바이트에 해당하며, 문자 스트림을 쓰고 읽을 때는 `FileWriter`와 `FileReader`를 이용한다.

## 기타 직렬화 방법
- 문자열 형태로 직렬화  
바이너리 프로토콜에 비해 인코딩과 디코딩 비용이 크다. 오버헤드가 훨씬 커서 CPU와 대역폭을 더 소모한다.  
그러나 개발 측면에서 편리하며 생산성을 높일 수 있다.
  - CSV: 데이터를 콤마로 구분하는 방법으로, 표 형태의 빅데이터 직렬화에 많이 사용된다.  
  **Apache Commons CSV**, **opencsv** 등의 라이브러리를 이용할 수 있다.
  - JSON: 구조적 데이터에 많이 사용되며, 자바스크립트에서 쉽게 사용할 수 있고 오버헤드가 적어 XML을 밀어내고 많이 사용된다.  
  **Jackson**, **GSON** 등의 라이브러리를 이용할 수 있다.
- 이진 형태로 직렬화  
다양한 바이너리 프로토콜이 존재하여 데이터 변환과 전송 방법 등이 제시된다.  
**Protocol Buffer**, **Apache Avro** 등이 있다.  
  
자바 개발자의 입장에서는 자바 직렬화, 문자열 직렬화, 이진 직렬화 순으로 개발이 편하다.  
성능은 대략 그 반대라고 볼 수 있다.  
(외부에서 [더 보기1][7], [더 보기2][8])

[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/AutoCloseable.md#try-with-resources
[2]: https://www.oracle.com/java/technologies/javase/seccodeguide.html
[3]: https://docs.oracle.com/en/java/javase/17/core/serialization-filtering1.html#GUID-55BABE96-3048-4A9F-A7E6-781790FF3480
[4]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/FilterInputStream.html
[5]: https://ducmanhphan.github.io/2019-01-07-Implementation-with-file-in-java/
[6]: https://j.mearie.org/post/122845365013/serialization
[7]: https://techblog.woowahan.com/2550/
[8]: https://nesoy.github.io/articles/2018-04/Java-Serialize
