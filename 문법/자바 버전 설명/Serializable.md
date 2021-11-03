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
import java.io.ObjectOutputStream;

try (
    FileOutputStream fout = new FileOutputStream("file.ser");
    ObjectOutputStream out = new ObjectOutputStream(fout);
) {
    ...
    out.writeObject(list);
    out.flush();
}
```
> **try-with-resources**: try( ... )의 괄호 안에 객체를 선언하면, try 문을 벗어날 때 자원을 자동으로 해제해준다.
