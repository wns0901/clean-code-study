# 클린 코드 10장: 클래스

## 핵심 원칙

### 1) 클래스는 작게, 작게, 더 작게!
- 클래스는 작을수록 좋다. 작다는 것은 **한 가지 책임(Single Responsibility Principle)** 을 의미한다.
- 한 클래스가 하나의 책임만 갖도록 설계하면, **변경이 필요한 이유도 하나**로 줄어들고, 유지보수가 쉬워진다.

```java
// 나쁜 예제: 너무 많은 책임을 가진 클래스
public class UserManager {
    public void addUser(User user) { /* ... */ }
    public void deleteUser(User user) { /* ... */ }
    public void sendEmail(User user, String message) { /* ... */ }
    public void generateReport() { /* ... */ }
}

// 좋은 예제: 책임을 분리한 클래스들
public class UserService {
    public void addUser(User user) { /* ... */ }
    public void deleteUser(User user) { /* ... */ }
}

public class EmailService {
    public void sendEmail(User user, String message) { /* ... */ }
}

public class ReportService {
    public void generateReport() { /* ... */ }
}
```

### 2) 클래스는 응집도(cohesion)를 높여야 한다
- 응집도란 **클래스의 모든 메서드와 필드가 밀접하게 연관되어 있는 정도**다.
- 높은 응집도를 가진 클래스는 **하나의 목적에 집중**하며, 그 목적과 관련된 기능만 포함한다.
- 응집도가 낮으면 클래스가 너무 많은 일을 하거나, 서로 관련 없는 메서드가 섞여 있다는 의미다.

### 3) SRP(단일 책임 원칙)를 지켜라
- 하나의 클래스는 **단 하나의 책임만 가져야 한다.**
- 책임이 명확하면 코드가 더 이해하기 쉬워지고, 수정도 간단해진다.
- SRP를 기준으로 클래스를 쪼개면 자연스럽게 작고 깔끔한 구조가 만들어진다.

### 4) 변경하기 쉬운 클래스를 만들자
- 클래스를 잘 설계하면 **수정이 필요한 부분만 최소한으로 변경**할 수 있다.
- 인터페이스를 활용해서 구현체와 분리하면, 나중에 구현체가 바뀌어도 호출부는 그대로 유지할 수 있다.

```java
public interface NotificationService {
    void notify(User user, String message);
}

public class EmailNotification implements NotificationService {
    public void notify(User user, String message) {
        // 이메일 전송 로직
    }
}

public class UserController {
    private NotificationService notificationService;
    
    public UserController(NotificationService notificationService) {
        this.notificationService = notificationService;
    }
    
    public void notifyUser(User user) {
        notificationService.notify(user, "Welcome!");
    }
}
```

### 5) 클래스 간의 결합도를 낮추자
- **결합도가 낮을수록 클래스가 독립적으로 동작할 수 있고, 변경에도 유연하게 대응할 수 있다.**
- 다른 클래스에 강하게 의존하지 않도록 인터페이스나 추상 클래스를 활용하자.

### 6) 순환 의존성을 피하자
- 클래스들이 서로를 의존하면, **변경할 때 모든 클래스를 함께 수정해야 하는 문제**가 생긴다.
- 순환 의존성을 피하려면 **중간 계층이나 의존성 주입(DI)**을 활용하자.

---

## 정리
- **작고 응집력 있는 클래스가 유지보수성과 확장성 모두에 유리하다.**
- **한 클래스는 한 가지 책임만 가져야 한다.**
- **응집도는 높이고, 결합도는 낮춰라.**
- **변경에 유연하도록 인터페이스를 활용하자.**
- **클래스 간 의존성은 명확하고 단방향으로 설계하자.**


