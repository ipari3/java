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
#### transient
트랜지언트 데이터는 직렬화에서 제외된다.  
주로 사회보장번호나 비밀번호 등의 보안 정보가 이에 해당한다.

## 직렬화 (Serialization)
객체의 상태를 바이트 스트림으로 변환하는 것.  
바이트 스트림은 데이터베이스로 저장되거나 네트워크로 전송될 수 있다.  
**역직렬화**(Deseriealization)는 그 반대 과정이다.
#### serialize
직렬화는 `ObjectOutputStream` 클래스로 구현한다.  
직렬화 결과를 파일로 저장할 때, 표준 확장자는 **.ser**이다.
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
> [**try-with-resources**][1]
#### deserialize
역직렬화는 `ObjectInputStream` 클래스로 구현한다.
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

[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/AutoCloseable.md#try-with-resources
