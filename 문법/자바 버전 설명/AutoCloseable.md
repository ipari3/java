## try-with-resources
AutoCloseable 객체를 자동으로 close해준다.  
close는 flush부터 수행하고 clear한다. (따라서 따로 flush할 필요가 없다.)
```
try (Statement stmt = con.createStatement()) {
    ...
} catch (SQLException e) {
    ...
}
```
> **flush**: 데이터를 write하면  버퍼에 저장된다.  
> flush는 버퍼의 데이터가 모두 쓰였는지 확인해준다.

## AutoClseable
AutoClseable를 구현한 클래스는 매우 많다.  
주로 -Channel, -Stream, -Reader, -Writer, -Loader, -Socket, -File의 이름을 갖는다.  
([외부 링크에서 더 보기][1])


[1]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/AutoCloseable.html
