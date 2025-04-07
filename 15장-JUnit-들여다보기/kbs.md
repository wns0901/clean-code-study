# 15장 JUnit 들여다보기
## JUnit을 리팩토링해보자
JUnit은 이미 깔끔하게 작성된 코드다.<br>
그러나 개선이 불필요한 모듈은 없다.<br>

## 주요 수정점
- 조건문 캡슐화
> 조건문의 의도는 빠르고 명확하게 표현되어야한다. 별도 메소드로 뽑아 캡슐화한다.
- 부정문 반전
> 부정문은 긍정문보다 이해가 어렵다고 한다.
- 기능에 적합한 이름
> 메소드, 클래스, 변수들은 명확하고 역할을 드러내는 이름이 필요하다.
- 단일 책임
> 두 가지 기능을 하는 메소드가 있다면 분리한다.
- 일관적이지 못한 함수 사용 방식
```java
private void compactExpectedAndActual() {
  findCommonPrefix();
  findCommonSuffix();
  compactExpected = compactString(expected);
  compactActual = compactString(actual);
}
```
>와 같이 리턴값이 있는 함수와 없는 함수를 섞으면 좋지 않다.  
예측하기 힘들기 때문.
```java
private void compactExpectedAndActual() {
  prefixlndex = findCommonPrefix();
  suffixlndex = findCommonSuffix();
  compactExpected = compactString(expected);
  compactActual = compactString(actual);
}
```
> 와 같이 명시적이고 일관성 있게 작성
- 숨겨진 시간 결합도
> findCommonSuffix()가 findCommonPrefix()의 실행 결과에 의존적이라 할 때, 코드 상에서 이를 눈치채기 힘들다.<br>
리턴으로 받은 값을 매개변수로 줄 수도 있지만 그 값의 존재 이유에 대해 의심할 수 있다.
**시간 결합도가 존재하는 메소드(재사용성과 유연성이 부족해도 되는 경우)는 선행 메소드 내부에서 후행 메소드를 호출한다.**

## 번복
책의 순서대로 리팩토링을 진행할 때, 일관성 운운하며 리턴값을 붙힌 메소드가 어느 순간 다시 void 메소드로 변화하는둥 이전 변경을 번복하는 것을 볼 수 있다.<br>
이는 리팩토링에서 흔한 일이다. 더 나은 방법을 찾아가는 시행착오에서 이전 방법에 얽매일 필요는 없다.