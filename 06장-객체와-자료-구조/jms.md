
클린 코드(Clean Code) 6장은 **"객체와 자료 구조"**에 대해 다루고 있으며,
객체 지향 프로그래밍에서 **객체와 자료 구조의 차이점을 이해하고 올바르게 활용하는 방법**을 설명한다. 

---

## 핵심 원칙

### 1) 객체와 자료 구조의 차이를 이해하라  
- **객체(Object)**: 데이터와 행위를 함께 포함하며, 내부 구현을 숨긴다.  
- **자료 구조(Data Structure)**: 데이터만 포함하고, 별도의 함수에서 데이터를 처리한다.  
- 객체는 **추상화**를 통해 캡슐화하고, 자료 구조는 **데이터를 직접 조작**하도록 설계된다.  

### 2) 자료 구조는 데이터를 공개하고, 객체는 숨긴다  
- 객체는 **데이터를 숨기고(캡슐화), 메서드를 통해 조작**하도록 설계해야 한다.  
- 반면 자료 구조는 **데이터를 직접 노출하고, 별도의 로직에서 처리**하는 것이 일반적이다.  

```java
// 객체 예시
public class Car {
    private String model;
    private int speed;
    
    public Car(String model) {
        this.model = model;
        this.speed = 0;
    }
    
    public void accelerate() {
        speed += 10;
    }
    
    public int getSpeed() {
        return speed;
    }
}
```
```java
// 자료 구조 예시
public class CarData {
    public String model;
    public int speed;
}
```
- 위의 예제에서 객체(Car)는 데이터를 숨기고 메서드를 통해 조작하지만, 자료 구조(CarData)는 데이터를 직접 접근할 수 있도록 노출한다.  

### 3) 절차적인 코드와 객체 지향 코드의 차이  
- **절차적인 코드**는 자료 구조를 사용하고, 함수에서 데이터를 처리한다.  
- **객체 지향 코드**는 객체가 스스로 데이터를 처리하도록 한다.  
- 두 접근 방식의 장단점을 이해하고, **상황에 따라 적절히 사용해야 한다.**  

### 4) 변경에 유연한 설계를 하라  
- 새로운 기능이 추가될 때, **기존 코드를 최소한으로 수정할 수 있도록** 설계해야 한다.  
- 객체를 사용할 때는 **캡슐화를 강화하고, 인터페이스를 활용**하면 변경에 유연해진다.  
- 자료 구조를 사용할 때는 **데이터를 직접 노출하는 대신, 변경 가능성을 고려한 구조로 설계**해야 한다.  

### 5) 데이터 전송 객체(DTO)와 활성 레코드 패턴  
- **DTO(Data Transfer Object)**: 단순한 데이터 컨테이너로, 로직을 포함하지 않는다.  
- **Active Record**: 데이터베이스와 직접 연결된 객체로, 데이터를 저장하고 조작하는 역할을 한다.  

```java
// DTO 예시
public class UserDTO {
    public String name;
    public int age;
}
```
```java
// Active Record 예시
public class User extends ActiveRecord {
    private String name;
    private int age;
    
    public void save() {
        // DB 저장 로직
    }
}
```

---


- **객체와 자료 구조의 차이를 명확히 이해하고, 적절한 곳에 활용하자.**  
- **객체는 데이터를 숨기고, 메서드를 통해 조작하도록 설계하자.**  
- **절차적인 코드와 객체 지향 코드의 장단점을 고려하여 유연한 설계를 하자.**  
- **DTO와 Active Record 패턴을 적절히 사용하여 데이터 관리의 효율성을 높이자.**  


