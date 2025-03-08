# 객체와 자료 구조

## 변수의 비공개 정의
- 변수를 비공개(private)로 정의하는 이유는 **외부에서 해당 변수에 직접 의존하지 않도록 하기 위함**이다.
- 캡슐화를 통해 내부 구현이 변경되더라도 외부 코드에 미치는 영향을 최소화할 수 있다.

## 자료 추상화
- 자료를 직접 공개하기보다는 추상적인 개념으로 표현하는 것이 좋다.
- 이유: **세부 사항이 변경되더라도 인터페이스만 유지되면 외부 코드가 영향을 받지 않는다.**

#### 예제 (좋은 자료 추상화)
```cpp
class Rectangle {
private:
    int width, height;

public:
    void setSize(int w, int h) {
        width = w;
        height = h;
    }
    int getArea() const {
        return width * height;
    }
};
```
- `width`와 `height`를 직접 공개하는 대신 `setSize()`와 `getArea()`를 통해서만 접근하도록 한다.

## 자료/객체 비대칭
- **객체(Object)**: 내부 자료를 숨기고, 데이터를 다룰 수 있는 함수(메서드)만 공개한다.
- **자료구조(Data Structure)**: 자료를 그대로 공개하고, 별다른 함수는 제공하지 않는다.
- **절차적 코드 vs 객체 지향 코드**
  - 절차적 코드: 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다.
  - 객체 지향 코드: 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.
  - 절차적 코드는 새로운 자료 구조를 추가하기 어렵다. (모든 함수를 수정해야 함)
  - 객체 지향 코드는 새로운 함수를 추가하기 어렵다. (모든 클래스를 수정해야 함)
  - **즉, 새로운 자료 타입이 필요한 경우 객체 지향이 적합하고, 새로운 함수가 필요한 경우 절차적 코드가 적합하다.**

  ## 디미터 법칙 (Law of Demeter)

### 1. 개념
- **디미터 법칙**은 모듈(클래스, 함수 등)이 **자신이 직접 조작하는 객체의 속성만 알아야 한다**는 원칙이다.
- 즉, **연쇄적인 메서드 호출을 피하고 직접적으로 필요한 정보만 요청해야 한다**.

#### 2. 예제 (잘못된 코드 - 기차 충돌)
```cpp
class Engine {
public:
    void start() {}
};

class Car {
public:
    Engine* getEngine() { return &engine; }
private:
    Engine engine;
};

void startCar(Car* car) {
    car->getEngine()->start(); // 연쇄적인 호출 (기차 충돌)
}
```
#### 3. 개선된 코드
```cpp
class Car {
public:
    void startEngine() { engine.start(); }
private:
    Engine engine;
};

void startCar(Car* car) {
    car->startEngine(); // 디미터 법칙 준수
}
```

---

## 잡종 구조 (Hybrid Structure)
- 절반은 객체, 절반은 자료 구조인 **잡종 구조**는 피해야 한다.

### 1. 특징
- 중요한 기능을 수행하는 함수가 있지만 **공개 변수 또는 조회/설정 함수가 존재**.
- 이는 객체와 자료 구조의 **단점만 모아놓은 구조**로, 유지보수 및 확장이 어렵다.

#### 2. 예제 (잘못된 잡종 구조)
```cpp
class Hybrid {
public:
    int data;
    void processData() { data *= 2; }
};
```
- `data`가 공개되어 있어, 외부에서 직접 조작할 수 있다.

#### 3. 개선된 코드
```cpp
class ProperEncapsulation {
private:
    int data;
public:
    void setData(int value) { data = value; }
    int getData() const { return data; }
    void processData() { data *= 2; }
};
```
- 비공개 변수와 적절한 인터페이스 제공.

---

## 자료 전달 객체 (DTO: Data Transfer Object)
- **DTO는 공개 변수만 있고 함수가 없는 데이터 구조체**이다.
- **데이터베이스 또는 API에서 정보를 주고받을 때 주로 사용**.

#### 1. 예제
```cpp
class UserDTO {
public:
    std::string name;
    int age;
};
```

- **빈(Bean) 객체**는 DTO와 유사하지만 **비공개 변수와 getter/setter를 포함**.

#### 2. 예제 (빈 객체)
```cpp
class UserBean {
private:
    std::string name;
    int age;
public:
    void setName(std::string n) { name = n; }
    std::string getName() const { return name; }
    void setAge(int a) { age = a; }
    int getAge() const { return age; }
};
```

---

## 결론
- **디미터 법칙을 준수하여 불필요한 연쇄적인 호출을 피해야 한다**.
- **잡종 구조는 유지보수 및 확장성이 떨어지므로 피하는 것이 좋다**.
- **DTO는 데이터를 전달하는 데 적합한 구조체이며, Bean 객체는 접근 제어를 강화한 형태로 사용된다**.
- **객체와 자료 구조의 차이를 이해하고, 적절한 설계를 통해 가독성과 유지보수성을 높이는 것이 중요하다**.
