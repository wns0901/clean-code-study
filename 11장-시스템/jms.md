# 클린 코드 11장: 시스템

## 핵심 원칙

### 1) 시스템 구축도 깨끗하게!
- 클린 코드는 **클래스나 함수 수준에만 국한되지 않는다.** 시스템 전체의 구조도 클린하게 설계되어야 한다.
- **시스템은 확장 가능하고 유연하며, 변경에 강해야 한다.**
- 복잡한 시스템일수록 구성 요소 간의 **결합도를 낮추고, 명확한 경계를 유지**하는 것이 중요하다.

### 2) 시스템 확장은 ‘설정’으로 분리하라
- 새로운 기능을 추가하거나 외부 라이브러리를 바꿀 때, 코드 자체를 수정하는 것이 아니라 **설정(config)** 을 통해 변경할 수 있도록 하자.
- **설정과 실행을 분리**하면 시스템 변경 시 코드 수정 없이 동작을 제어할 수 있다.

```java
// 나쁜 예제: 코드에 구현이 박혀 있음
public class Database {
    public Connection getConnection() {
        return DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb");
    }
}
```

```java
// 좋은 예제: 설정 분리를 통해 유연한 변경 가능
public class Database {
    private String dbUrl;

    public Database(String dbUrl) {
        this.dbUrl = dbUrl;
    }

    public Connection getConnection() {
        return DriverManager.getConnection(dbUrl);
    }
}
```

### 3) 의존성 주입(Dependency Injection) 활용하기
- 의존성 주입은 **컴포넌트 간의 결합도를 낮추고, 테스트와 유지보수를 쉽게 만든다.**
- 시스템이 커질수록 **객체 생성과 의존성 연결을 분리**해야 한다.
- 직접 객체를 생성하지 말고, 외부에서 주입받도록 하자.

```java
// 의존성을 내부에서 생성하는 나쁜 예
public class OrderService {
    private final PaymentService paymentService = new PaymentService();
}

// 외부에서 의존성을 주입받는 좋은 예
public class OrderService {
    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### 4) 시스템은 아키텍처와 경계를 명확히 하자
- 시스템 구성 요소는 **관심사를 분리**하여, UI, 비즈니스 로직, 데이터 접근 등이 서로 명확히 구분되어야 한다.
- 이런 분리를 통해 시스템은 **확장성과 변경에 강한 구조**를 갖게 된다.

### 5) 깨끗한 시스템 설계는 테스트로 검증된다
- 시스템 단위의 테스트도 중요하다. 단위 테스트뿐 아니라, **통합 테스트와 시스템 테스트**를 통해 전체 동작을 검증하자.
- 테스트 가능한 시스템을 설계하려면 **느슨한 결합과 명확한 인터페이스**가 필수다.

---

## 정리
- **시스템 전체도 클린 코드 원칙을 따라야 한다.**
- **설정과 실행을 분리하면 시스템 변경이 쉬워진다.**
- **의존성 주입을 통해 결합도를 낮추고 유연한 시스템을 만들자.**
- **관심사 분리와 명확한 아키텍처가 확장 가능한 시스템을 만든다.**
- **시스템도 테스트 가능하도록 설계하고, 실제 테스트를 통해 검증하자.**
