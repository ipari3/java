**람다식**(람다 표현식 혹은 람다 함수라고도 함)은 익명 함수(Anonymous functions)를 지칭한다.
- 장점: 코드의 간결성(루프문 제거, 함수 재활용), 지연 연산(스트리밍 혹은 체인이라고도 함)으로 필요한 정보만 사용하여 퍼포먼스 향상
- 단점: 남용 시 가독성 저하, 모든 원소를 순회하는 경우는 오히려 느림 등
  
(예시) for문  
모든 원소를 순회하므로 람다식이 더 느린 경우이다.
```
// for 문
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// 람다식
IntStream.range(0, 10).forEach((int value) -> System.out.println(value)); // 기본 형태
IntStream.range(0, 10).forEach(value -> System.out.println(value)); // 파라미터 자료형 생략. 컴파일러가 추론 가능.
IntStream.range(0, 10).forEach(System.out::println); // 메서드 참조 이용.
```
람다식이 단 하나의 메소드만 호출하는 경우, 메소드 참조를 이용하면 매개변수를 명시하지 않아도 된다.  
형식의 위의 예시대로 ::를 이용하며, 매개변수가 사라진다.  
::의 좌측에는 클래스나 참조변수를 넣고 우측에는 메소드를 넣는다. (ClassName::methodName)
