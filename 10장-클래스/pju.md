# 클래스 설계 원칙

## 클래스 체계

- **변수 목록 순서**
  1. 정적 공개 상수 (가장 먼저)
  2. 정적 비공개 변수
  3. 비공개 인스턴스 변수
- **공개 변수는 거의 필요 없다**

---

## 1. 클래스는 작아야 한다

- 클래스는 **작아야 한다.**
- 클래스 설명은 `if`, `and`, `or`, `but` 없이 **25단어 내외**로 가능해야 한다.

---

## 2. 단일 책임 원칙 (SRP: Single Responsibility Principle)

- 클래스나 모듈은 **변경할 이유가 하나뿐**이어야 한다.
- **SRP는 객체 지향 설계에서 중요한 개념**이다.
- 대부분 개발자들은 동작하는 코드에만 집중하지만,  
  **깨끗하고 체계적인 소프트웨어**로 관심을 전환해야 한다.
- **큰 클래스 몇 개보다 작은 클래스 여러 개**가 더 바람직하다.

---

## 3. 응집도 (Cohesion)

- **인스턴스 변수 수는 적을수록 좋다.**
- 클래스 메서드는 **하나 이상의 인스턴스 변수**를 사용해야 한다.
- 메서드가 더 많은 인스턴스 변수를 사용할수록 **응집도가 높아진다.**

---

### ✅ 예시: 응집도가 높은 클래스
```java
class Point {
    private int x;
    private int y;

    public void move(int dx, int dy) {
        x += dx;
        y += dy;
    }

    public double distanceFromOrigin() {
        return Math.sqrt(x * x + y * y);
    }
}
```
- `move`, `distanceFromOrigin` 모두 `x`, `y`를 사용 → **응집도 높음**

---

### ⚠️ 예시: 응집도를 잃은 클래스 → 분리 필요
```java
class UserManager {
    private Database db;
    private Logger logger;

    public void saveUser(User user) {
        db.save(user);
    }

    public void logUserActivity(String activity) {
        logger.log(activity);
    }
}
```
- `db`, `logger`는 서로 다른 책임 →  
  `UserManager`를 `UserRepository`, `UserLogger`로 **분리**하는 것이 적절함

---

## 3-1. 응집도를 유지하면 작은 클래스가 많이 생긴다

- 클래스가 **응집력을 잃는다면 과감히 쪼개라!**

---

## 4. 변경하기 쉬운 클래스

- **기능 추가나 수정 시, 건드릴 코드가 최소**인 구조가 바람직하다.

## 결론

클래스는 작고 명확한 책임을 가져야 하며, 높은 응집도와 낮은 결합도를 유지해야 유지보수가 쉬운 구조로 발전할 수 있다. 테스트 가능한 구조, 명확한 책임 분리, 그리고 일관된 설계 원칙을 통해 소프트웨어는 더 견고하고 변화에 유연하게 대응할 수 있다.