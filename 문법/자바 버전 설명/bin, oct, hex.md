### 리터럴
```
int bin = 0b1010; // print: 10
int oct = 012; // print: 10
int hex = 0xa; // print: 10
```
맨 앞의 문자는 숫자 0이다.  
그 뒤에 붙는 b나 x는 대소문자 모두 가능하다.

### 문자열
```
// int to String
String bin = Integer.toBinaryString(10); // print: 1010
String oct = Integer.toOctalString(10); // print: 12
String hex = Integer.toHexString(10); // print: a

// String to int
Integer.valueOf(bin, 2)); // print: 10
Integer.valueOf(oct, 8)); // print: 10
Integer.valueOf(hex, 16)); // print: 10
```
