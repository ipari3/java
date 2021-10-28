## 람다식 (람다 표현식, 람다 함수)
익명 함수(Anonymous functions)를 지칭한다.
#### 장점
루프문을 제거하고 함수를 재활용할 수 있어 코드가 간결해진다.  
또한 [지연 연산][1]으로 필요한 정보만 사용하여 퍼포먼스를 향상시킨다.
#### 단점
남용 시 가독성이 저하되고, 모든 원소를 순회하는 경우는 오히려 일반 루프보다 느리다.
```
// for문
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// 람다식 (모든 원소를 순회하므로 람다식이 더 느린 경우이다.)
IntStream.range(0, 10).forEach((int value) -> System.out.println(value)); // 기본 형태
IntStream.range(0, 10).forEach(value -> System.out.println(value)); // 파라미터 자료형 생략. 컴파일러가 추론 가능.
IntStream.range(0, 10).forEach(System.out::println); // 메소드 참조 이용.
```
## 메소드 참조
람다식이 단 하나의 메소드만 호출하는 경우, 메소드 참조(method reference)를 이용하면 매개변수를 명시하지 않아도 된다.  
double colon `::`을 이용하여 ClassName::methodName의 형식으로 작성다.

[1]: https://github.com/ipari3/java/blob/main/%EB%AC%B8%EB%B2%95/%EC%9E%90%EB%B0%94%20%EB%B2%84%EC%A0%84%20%EC%84%A4%EB%AA%85/Stream.md#%EA%B2%8C%EC%9C%BC%EB%A5%B8-%EB%B0%9C%EB%8F%99-lazy-invocation
