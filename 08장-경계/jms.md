# 클린 코드 8장: 경계

## 핵심 원칙

### 1) 외부 코드와의 경계를 명확히 하라
- 우리가 작성하는 코드가 항상 독립적인 것은 아니다. **외부 라이브러리나 API를 사용할 때 경계를 명확히 관리하는 것이 중요하다.**
- 외부 코드에 직접 의존하면 변화에 취약해지므로 **적절한 인터페이스를 정의하여 의존성을 줄이자.**

```java
// 나쁜 예제: 외부 라이브러리에 직접 의존
List<String> users = new ArrayList<>();
users.add("Alice");
```

```java
// 좋은 예제: 인터페이스를 활용한 경계 설정
public interface UserRepository {
    void addUser(String user);
    List<String> getUsers();
}

public class UserRepositoryImpl implements UserRepository {
    private final List<String> users = new ArrayList<>();
    
    @Override
    public void addUser(String user) {
        users.add(user);
    }
    
    @Override
    public List<String> getUsers() {
        return new ArrayList<>(users);
    }
}
```

### 2) 외부 라이브러리는 최소한의 부분만 노출하라
- 외부 라이브러리를 그대로 사용하면 **변경이 발생할 때 우리 코드가 함께 변경되어야 하는 문제**가 생긴다.
- 따라서 **중간 계층(Wrapper)을 만들어 라이브러리 변경 시 영향을 최소화하자.**

```java
// 나쁜 예제: 외부 라이브러리 직접 사용
Map<String, Object> json = new HashMap<>();
json.put("name", "Alice");
json.put("age", 25);
```

```java
// 좋은 예제: 중간 계층을 활용
public class UserJson {
    private final Map<String, Object> data = new HashMap<>();
    
    public void setName(String name) {
        data.put("name", name);
    }
    
    public void setAge(int age) {
        data.put("age", age);
    }
    
    public Map<String, Object> getData() {
        return new HashMap<>(data);
    }
}
```

### 3) 테스트하기 쉽게 설계하라
- **경계를 명확히 하면 테스트가 용이해진다.**
- 외부 API나 라이브러리를 직접 호출하지 않고 **인터페이스를 사용하면 목(Mock) 객체를 활용한 테스트가 가능해진다.**

```java
// 예제: 인터페이스를 활용한 테스트 가능 코드
public interface PaymentGateway {
    boolean processPayment(double amount);
}

public class PaymentService {
    private final PaymentGateway paymentGateway;
    
    public PaymentService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }
    
    public boolean pay(double amount) {
        return paymentGateway.processPayment(amount);
    }
}
```

```java
// 테스트 코드에서 목(Mock) 객체 사용
class MockPaymentGateway implements PaymentGateway {
    @Override
    public boolean processPayment(double amount) {
        return true;
    }
}

@Test
void testPayment() {
    PaymentService service = new PaymentService(new MockPaymentGateway());
    assertTrue(service.pay(100.0));
}
```

### 4) 외부 API 변경에 유연하게 대응하라
- **외부 API는 언제든 변경될 수 있다.**
- 이런 변경을 대비하여 **적절한 추상화를 도입하면 유지보수 비용을 줄일 수 있다.**

```java
// 나쁜 예제: 직접 API 사용
public class EmailSender {
    public void sendEmail(String recipient, String message) {
        ExternalEmailService.send(recipient, message);
    }
}
```

```java
// 좋은 예제: 인터페이스를 활용하여 외부 API 변경에 대비
public interface EmailService {
    void sendEmail(String recipient, String message);
}

public class EmailServiceImpl implements EmailService {
    @Override
    public void sendEmail(String recipient, String message) {
        ExternalEmailService.send(recipient, message);
    }
}
```

---

## 정리
- **외부 라이브러리나 API와의 경계를 명확히 관리하자.**
- **중간 계층(Wrapper)을 만들어 직접적인 의존성을 줄이자.**
- **인터페이스를 활용하여 테스트 가능성을 높이자.**
- **외부 API 변경에 대비할 수 있도록 적절한 추상화를 도입하자.**

