# 클린 코드 15장: JUnit 들여다보기


---

## 1) 테스트 코드로부터 알아낸 사실

JUnit 내부의 `ComparisonCompactorTest` 클래스는  
`ComparisonCompactor`의 기능을 철저히 검증하는 **정교한 테스트 케이스**들로 구성되어 있다.

- 테스트는 다음을 확인한다:
  - 생성자의 파라미터 역할 (앞뒤 컨텍스트 길이, 예상 값, 실제 값)
  - `compact()` 메서드의 메시지 출력 형식
  - 공통 접두어/접미어 처리
  - 문자열 차이를 `[...]`로 표시하는 방식
  - `null` 값 처리

- **테스트 커버리지는 100%**
  - 모든 조건문, 분기, 경계 상황을 철저히 테스트
  - `if`, `for`, `null`, 중복 문자 등 예외 케이스까지 포함

> 테스트가 완벽할수록, 리팩터링 시 안심할 수 있다.

---

## 2) 그래서 개선해봤다: 보이스카우트 규칙 적용

JUnit의 코드도 잘 짜여 있지만, **“처음보다 조금 더 깨끗하게” 만들 수는 있다.**  
그래서 다음과 같은 리팩터링 포인트들이 제시된다:

### 인코딩 피하기 
- 멤버 변수 접두어 `f` 제거 → 현대 IDE에서는 불필요
```java
// before
private String fExpected;

// after
private String expected;
```

### 조건 캡슐화
- 복잡한 조건을 메서드로 추출해서 의도 명확히 표현
```java
// before
if (expected == null || actual == null || areStringsEqual())

// after
if (shouldNotCompact())

private boolean shouldNotCompact() {
    return expected == null || actual == null || areStringsEqual();
}
```

### 표준 명명법 사용
- 멤버 변수와 지역 변수의 이름 충돌 방지
```java
String compactExpected = compactString(expected);
String compactActual = compactString(actual);
```

### 부정 조건 제거
- !조건 대신 긍정 조건으로 전환하여 가독성 향상
```java
private boolean canBeCompacted() {
    return expected != null && actual != null && !areStringsEqual();
}
```

### 함수 이름으로 부수 효과 드러내기 
- 단순한 압축이 아니라 포맷을 포함하므로 이름 변경
```java
public String formatCompactedComparison(String message)
```

### 함수는 한가지 일만 하게 
- 압축과 출력 분리
```java
private void compactExpectedAndActual() {
    findCommonPrefix();
    findCommonSuffix();
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
}
```

### 일관성 있게 설계
- 메서드들이 값을 반환하는 방식 통일
- prefix, suffix를 index -> length로 명명 변경

### 시각적 결합 제거 
### 경계조건 캡슐화
### 죽은 코드 제거 

