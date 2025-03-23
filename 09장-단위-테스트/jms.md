# 클린 코드 9장: 단위 테스트

## 핵심 원칙

### 1) 좋은 단위 테스트의 중요성
- 단위 테스트(Unit Test)는 코드의 **품질을 유지하고, 변경에 대한 신뢰도를 높여준다.**
- 좋은 단위 테스트는 **빠르고, 독립적이며, 반복 가능해야 한다.**

### 2) 테스트 코드도 클린 코드처럼 유지하라
- 테스트 코드도 유지보수 대상이므로 **가독성과 유지보수성을 고려하여 작성해야 한다.**
- 중복을 피하고, 명확한 테스트 구조를 유지하자.

```java
// 나쁜 예제: 중복 코드가 많고 가독성이 떨어짐
@Test
public void testAddUser() {
    User user = new User("Alice");
    userRepository.add(user);
    User retrievedUser = userRepository.findByName("Alice");
    assertNotNull(retrievedUser);
    assertEquals("Alice", retrievedUser.getName());
}

@Test
public void testRemoveUser() {
    User user = new User("Bob");
    userRepository.add(user);
    userRepository.remove(user);
    assertNull(userRepository.findByName("Bob"));
}
```

```java
// 좋은 예제: 공통 로직을 메서드로 분리하여 가독성을 높임
private User createAndAddUser(String name) {
    User user = new User(name);
    userRepository.add(user);
    return user;
}

@Test
public void testAddUser() {
    User user = createAndAddUser("Alice");
    assertNotNull(userRepository.findByName("Alice"));
    assertEquals("Alice", user.getName());
}

@Test
public void testRemoveUser() {
    User user = createAndAddUser("Bob");
    userRepository.remove(user);
    assertNull(userRepository.findByName("Bob"));
}
```

### 3) 테스트는 F.I.R.S.T 원칙을 따라야 한다
- **Fast(빠르게 실행되어야 한다)**: 테스트가 느리면 실행 빈도가 줄어들어 효과가 떨어진다.
- **Independent(독립적이어야 한다)**: 테스트 간에 의존성이 있으면 특정 테스트가 실패할 때 원인을 찾기 어렵다.
- **Repeatable(반복 가능해야 한다)**: 테스트는 환경에 의존하지 않고 언제든지 동일한 결과를 내야 한다.
- **Self-Validating(자체적으로 검증 가능해야 한다)**: 테스트 결과는 논리적인 조건문이 아니라, `assert` 문을 통해 자동으로 검증되어야 한다.
- **Timely(적시에 작성해야 한다)**: 테스트는 개발과 함께 작성되어야 하며, 사후에 추가하면 코드 품질 유지가 어려워진다.

### 4) 테스트 커버리지를 높이는 방법
- 테스트 커버리지는 **코드의 실행 여부를 측정하는 지표**이지만, 100% 커버리지가 항상 좋은 것은 아니다.
- **중요한 로직과 변경 가능성이 높은 부분을 우선적으로 테스트하자.**
- **엣지 케이스(예외 상황)를 포함하여 테스트를 작성하자.**

```java
@Test
void testDivideByZero() {
    ArithmeticException thrown = assertThrows(ArithmeticException.class, () -> {
        calculator.divide(10, 0);
    });
    assertEquals("0으로 나눌 수 없습니다.", thrown.getMessage());
}
```

### 5) 테스트 더블(Test Double) 활용하기
- 실제 의존성을 제거하고 단위 테스트의 신뢰성을 높이기 위해 **Mock 객체**를 활용하자.
- 대표적인 테스트 더블 종류:
  - **Stub**: 미리 정해진 값을 반환하는 객체
  - **Mock**: 특정 동작을 검증하는 객체
  - **Fake**: 간단한 구현체로 대체하는 객체

```java
// Mockito를 사용한 Mock 객체 예제
@Test
void testSendEmail() {
    EmailService emailService = mock(EmailService.class);
    when(emailService.sendEmail("test@example.com", "Hello")).thenReturn(true);
    
    EmailSender sender = new EmailSender(emailService);
    assertTrue(sender.send("test@example.com", "Hello"));
}
```

---

## 정리
- **단위 테스트는 코드 품질을 유지하고 변경을 안전하게 만든다.**
- **테스트 코드도 클린 코드처럼 유지보수 가능하게 작성해야 한다.**
- **F.I.R.S.T 원칙을 지켜 테스트의 신뢰성을 높이자.**
- **테스트 커버리지를 높이되, 중요한 로직과 엣지 케이스를 우선적으로 검증하자.**
- **Mock과 같은 테스트 더블을 활용해 독립적인 단위 테스트를 작성하자.**

