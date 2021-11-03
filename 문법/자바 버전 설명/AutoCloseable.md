## try-with-resources
AutoCloseable 객체를 자동으로 close해준다.  
```
try (Statement stmt = con.createStatement()) {
    ...
} catch (SQLException e) {
    ...
}
```

## AutoClseable
AutoClseable를 구현한 클래스는 매우 많다.  
주로 -Channel, -Stream, -Reader, -Writer, -Loader, -Socket, -File의 이름을 갖는다.
([외부 링크에서 더 보기][1])


[1]: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/AutoCloseable.html
