# 이경원

## 객체와 자료 구조

---

## private
- 비공개로 정의하는 이유 → 남들이 변수에 의존하지 않게 만들고 싶어서
- 그렇다면 왜 getter와 setter 함수를 public으로 만들어서 비공개 변수를 외부에 노출할까?? (이 부분 정말 궁금했음..)
  - 가능하면 추상적 메서드를 제공

<br>

## 자료 추상화

```java
public class Point {
    public double x;
    public double y;
}
```

```java
public interface Point {
    double getX();
    double getY();
    void setCharesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```

- 차이점 
  - 인터페이스
    - 자료 구조를 명백하게 표현
    - 클래스 메서드가 접근 정책을 강제 
    - 하지만 좌표를 설정할 때는 두 값을 한꺼번에 설정
  - 클래스
    - 구현을 노출
- 구현을 감추려면 추상화가 필요
- 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 것이 좋다

<br>

## 자료/객체 비대칭
- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개

```java
public class Square {
    public Point topLeft;
    public double side;
}

public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle {
    public Point center;
    public double radius;
}

public class Geometry {
    public final double PI = 3.141592653589793;

    public double area(Object shape) throws NotSuchException
    {
        if (shape instanceof Square) {
            Square s = (Square) shape;
            return s.side*s.side;
        }
        else if (shape instanceof Rectangle) {
            Rectangle r = (Rentangle) shape;
            return r.height*r.width;
        }
        else if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return PI*c.radius*c.radius
        }
        throw new NoSuchShapeException();
    }
}
```

⬇️⬇️⬇️

```java
public class Square implements Shape {
    private Point topLeft;
    private double side;

    public double area() {
        return side*side;
    }
}

public class Rectangle implements Shape {
    private Point topLeft;
    private double height;
    private double width;

    public double area() {
        return height * width;
    }
}

public class Circle implement Shape {
    private Point center;
    private double radius;
    public final double PI = 3.141592653589793;

    public double area() {
        return PI * radius * radius;
    }
}
```

- 위 코드는 함수를 추가한다면 클래스는 아무 영향을 받지 않는다.
- 하지만 새로운 도형을 추가한다면 클래스에 속한 함수를 모두 고쳐야 한다.
- 절차적인 코드는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽다.
- 절차적인 코드는 새로운 자료 구조를 추가하기 어렵다.
- 객체 지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.
- 즉, 객체 지향 코드 -> 어려운 변경은 절차적인 코드에서 쉬우며, 절차적인 코드에서 어려운 변경은 객체 지향에서 쉽다.


<br>

## 디미터 법칙
> 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙

- 객체는 자료를 숨기고 함수를 공개
- 낯선 사람은 경계하고 친구랑만 놀라는 의미

#### 기차 충돌

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

- 여러 객차가 한 줄로 이어진 기차처럼 보인다.

⬇️⬇️⬇️

```java
Option opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

⬇️⬇️⬇️

```java
final String outputDir = ctxt.options.scratchDir.absolutePath;
```

- 자료 구조는 무조건 함수 없이 공개 변수만 포함하고 객체는 비공개 변수와 공개 함수를 포함해야 한다.

<br>

## 잡종 구조
- 자료구조와 객체가 반반씩 섞인 구조
- 양쪽의 단점을 모아놓은 구조이기 때문에 피해야 한다.

<br>

## 구조체 감추기

```java
ctxt.getAbsolutePathOfScratchDirectoryOption();

ctxt.getScretchDirectoryOption().getAbsolutePath();
```

- 첫 번째 코드는 ctxt 객체에 공개해야 하는 메서드가 너무 많아짐.
- 두 번째 코드는 `getScratchDirectoryOption()`이 객체가 아니라 자료 구조를 반환한다고 가정한다.

⬇️⬇️⬇️

- ctxt 객체에 임시 파일을 생성하라고 시킨다면?

```java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

- ctxt는 내부 구조를 드러내지 않으며, 모듈에서 해당 함수는 자신이 몰라야 하는 여러 객체를 탐색할 필요 X

<br>

## 자료 전달 객체
- 자료 구조체의 전형적인 형태는 공개 변수만 있고 함수가 없는 클래스 → 자료 전달 객체

#### DTO
> 데이터베이스에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 가장 처음 단계

#### 빈(bean) 구조
- private 변수를 조회/설정 함수로 조작

<br>

## 활성 레코드
- DTO의 특수한 형태
- 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과
- 활성 레코드를 사용할 때 잡종 구조가 나오지 않게 하려면 자료 구조로 취급해야 한다.

<br>

## 결론
- 객체 
  - 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기 쉬움
  - 기존 객체에 동작을 추가하기는 어려움
  - 새로운 자료 타입을 추가하는 유연성이 필요할 때
- 자료 구조 
  - 기존 자료 구조에 새 동작을 추가하기는 쉬움
  - 기존 함수에 새 자료 구조를 추가하기는 어려움
  - 새로운 동작을 추가하는 유연성을 필요할 때

<br>

## 정리
| 항목 | 설명 |
|-|-|
| private | 외부 의존을 막기 위해서 변수는 비공개하지만 getter/setter 사용 시 다시 노출 |
| 자료 추상화 | 구현을 감추고 인터페이스를 통해 자료를 조작하는 방식이 좋음 |
| 자료/객체 비대칭 | - 절차적 코드 : 새로운 연산(함수) 추가는 쉽지만, 새로운 데이터 타입 추가가 어려움. <br> - 객체 지향 코드: 새로운 데이터 타입 추가는 쉽지만, 기존 객체에 연산을 추가하는 것은 어려움. |
| 디미터 법칙 | 메서드가 반환한 객체의 메서드를 연쇄적으로 호출하는 것을 피하라 |
| 기차 충돌 | 메서드 체이닝(다단계 호출)을 피하고, 필요한 데이터를 반환하는 메서드를 직접 제공 |
| 잡종 구조 | 객체와 자료 구조를 섞으면 유지보수성이 나빠지므로 피해야 함 |
| 구조체 감추기 | 내부 데이터를 직접 노출하지 않고, 메서드를 통해 데이터를 제공하는 방식이 바람직 |
| DTO | 데이터 전송 용도의 객체. 보통 비즈니스 로직을 포함하지 않으며, 계층 간 데이터 전달을 위해 사용 |
| 활성 레코드 | DTO와 다르게 데이터베이스와 직접 연결된 객체. 비즈니스 로직을 포함할 수 있지만, 잘못 사용하면 유지보수성이 떨어질 수 있음 |
| 객체 vs 자료 구조 | - 객체 지향: 새로운 데이터 타입(클래스) 추가가 쉬움, 기존 클래스에 새로운 동작 추가가 어려움. <br> - 절차적 코드: 새로운 동작(함수) 추가가 쉬움, 새로운 데이터 타입 추가가 어려움. <br> - 유연성이 필요한 방향에 따라 방식을 선택해야 함.|