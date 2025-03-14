# 오류 처리와 깨끗한 코드

깨끗한 코드와 오류 처리는 밀접한 연관성이 있다.

## 1. 오류 코드보다 예외를 사용하라

### 1-1. Try-Catch-Finally 문부터 작성

- 예외가 발생할 가능성이 있는 코드를 작성할 때, `try-catch-finally` 문으로 시작하는 것이 좋다.
- `try-catch` 구조로 범위를 정의한 후, TDD를 사용하여 필요한 나머지 논리를 추가한다.
- 논리는 `FileInputStream`을 생성하는 코드와 `close` 호출문 사이에 위치하며, 오류나 예외가 전혀 발생하지 않는다고 가정한다.
- 강제로 예외를 발생시키는 테스트 케이스를 작성한 후 테스트를 통과하도록 코드를 작성하는 방법을 권장한다.

####  안 좋은 예 (예외 처리 없음)
```java
public int divide(int a, int b) {
    return a / b; // b가 0일 경우 예외 발생
}
```

####  좋은 예 (예외 처리 추가)
```java
public int divide(int a, int b) {
    try {
        return a / b;
    } catch (ArithmeticException e) {
        System.out.println("0으로 나눌 수 없습니다.");
        return 0; // 기본값 반환
    }
}
```

### 1-2. 미확인 예외를 사용하라

- 확인된 예외(Checked Exception)는 과거에는 좋은 아이디어였지만, 지금은 반드시 필요한 요소는 아니다.
- 확인된 예외는 **OCP(Open-Closed Principle, 개방-폐쇄 원칙) 위반**의 위험이 있다.
- 모든 함수가 최하위 함수에서 던지는 예외를 알아야 하므로 캡슐화가 깨진다.

### 1-3. 예외에 의미를 제공하라

- 예외 메시지는 오류 원인을 명확하게 설명해야 한다.
- 예외 메시지를 읽는 개발자가 문제를 빠르게 이해할 수 있도록 구체적으로 작성한다.

```java
throw new IllegalArgumentException("사용자 ID는 null이 될 수 없습니다.");
```

### 1-4. 호출자를 고려해 예외 클래스를 정의하라

#### 오류 분류 방법
- **오류가 발생한 컴포넌트로 분류**
- **유형으로 분류** (예: 디바이스 실패, 네트워크 실패, 프로그래밍 오류 등)
- 관심사는 **오류를 잡아내는 방법**이 되어야 한다.

### 1-5. 정상 흐름을 정의하라

#### 특수 사례 패턴 (Special Case Pattern)
- 클래스를 만들거나 객체를 조작하여 특수 사례를 처리하는 방식
- 예외적인 상황을 캡슐화하여 처리하는 패턴

```java
public class NullUser extends User {
    public NullUser() {
        super("", "");
    }
    @Override
    public boolean isNull() {
        return true;
    }
}
```

### 1-6. null을 반환하지 마라
- `null`을 반환하는 코드는 불필요한 작업을 증가시키고, 호출자에게 문제를 떠넘긴다.
- `Collections.emptyList()` 같은 안전한 대안을 사용하라.

```java
public List<String> getUsers() {
    return Collections.emptyList();
}
```

### 1-7. null을 전달하지 마라
- `null`을 전달하는 것은 오류를 초래할 가능성이 높다.
- 대신 `assert`를 활용하여 유효성 검사를 수행할 수 있다.

```java
public void setUser(User user) {
    assert user != null : "User 객체는 null일 수 없습니다.";
    this.user = user;
}
```

## 결론
 예외 처리는 코드의 가독성과 유지보수성을 높이는 중요한 요소이다.
 그리고 `try-catch-finally` 문을 적극적으로 활용하고, 의미 있는 예외 메시지를 제공하는 것이 좋다?
 `null`을 반환하거나 전달하지 말고, 안전한 대안을 고려하라. 그러나 과연 깨끗한 코드와 안전성이 있는 코드를 두마리 토끼를 잡을 수 있을지 의문이다.
 만약 둘 중 하나 택하라면 안전성에 나는 투표하고 싶다는 생각을 했다.
