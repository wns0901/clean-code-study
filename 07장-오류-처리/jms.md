# 클린 코드 7장: 오류 처리

## 핵심 원칙

### 1) 오류 처리는 프로그램의 가독성을 해치지 않도록 하라  
- 깨끗한 코드를 유지하려면 **오류 처리 코드가 프로그램의 핵심 로직을 방해하지 않아야 한다.**  
- try-catch 블록이 코드의 흐름을 어지럽히지 않도록 **적절한 예외 클래스를 정의하고, 오류를 처리하는 책임을 분리**하는 것이 중요하다.  

```java
// 나쁜 예제: 비즈니스 로직과 예외 처리 코드가 섞여 있음
public void process() {
    try {
        // 주요 로직
        int result = 10 / 0; // 오류 발생
    } catch (ArithmeticException e) {
        System.out.println("오류 발생: " + e.getMessage());
    }
}
```

```java
// 좋은 예제: 오류 처리 책임을 분리함
public void process() {
    try {
        executeLogic();
    } catch (CustomException e) {
        handleError(e);
    }
}

private void executeLogic() throws CustomException {
    // 주요 로직
    if (someConditionFails()) {
        throw new CustomException("문제 발생");
    }
}

private void handleError(CustomException e) {
    // 오류 처리 로직
    logError(e);
}
```

### 2) 예외는 논리적인 흐름을 유지하는 데 사용하라  
- **오류 코드 반환 방식보다는 예외를 활용하는 것이 좋다.**  
- 예외 처리를 하면 프로그램의 흐름을 **더 직관적으로 유지할 수 있다.**  

```java
// 나쁜 예제: 오류 코드를 반환하는 방식
public int divide(int a, int b) {
    if (b == 0) {
        return -1; // 오류 코드
    }
    return a / b;
}
```

```java
// 좋은 예제: 예외를 활용한 방식
public int divide(int a, int b) throws ArithmeticException {
    if (b == 0) {
        throw new ArithmeticException("0으로 나눌 수 없습니다.");
    }
    return a / b;
}
```

### 3) 체크 예외와 언체크 예외를 적절히 사용하라  
- **체크 예외(Checked Exception)**: 호출한 곳에서 반드시 처리해야 하는 예외. (예: `IOException`)
- **언체크 예외(Unchecked Exception)**: 실행 중 발생할 수 있는 예외로, 반드시 처리할 필요는 없음. (예: `NullPointerException`)
- **비즈니스 로직과 밀접한 오류는 체크 예외로, 예측 불가능한 오류는 언체크 예외로 처리하자.**  

### 4) 의미 있는 예외 메시지를 제공하라  
- 예외 메시지는 **오류의 원인과 해결 방법을 명확히 전달해야 한다.**  

```java
throw new IllegalArgumentException("입력 값이 유효하지 않습니다: " + input);
```

### 5) 오류 처리는 한 곳에서 집중적으로 관리하라  
- 여러 곳에서 예외 처리를 중복하지 말고, **일관된 방식으로 예외를 처리하는 전략을 세우자.**  
- 예외를 로깅하고 사용자에게 적절한 메시지를 제공하는 구조를 만들면 유지보수성이 높아진다.  

### 6) Null을 반환하지 말고, Optional을 사용하라  
- **Null을 반환하면 NullPointerException의 위험이 커진다.**  
- 대신 **Optional**을 활용하면 더 안전한 코드가 된다.  

```java
// Optional을 활용한 방식
public Optional<User> findUserById(int id) {
    return Optional.ofNullable(userRepository.findById(id));
}
```

---

## 정리
- **오류 처리는 프로그램의 가독성을 해치지 않도록 설계해야 한다.**  
- **예외를 활용하여 프로그램의 흐름을 논리적으로 유지하자.**  
- **체크 예외와 언체크 예외를 적절히 구분하여 사용하자.**  
- **명확한 예외 메시지를 제공하여 디버깅을 쉽게 만들자.**  
- **Null 반환을 피하고, Optional을 활용하여 안정적인 코드를 작성하자.**  


