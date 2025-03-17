# 이경원

## 오류 처리

---

## 오류 코드보다 예외를 사용하라

- 논리가 오류 처리 코드와 뒤섞이지 않기 위해서는 오류가 발생하면 **예외를 던지는 편**이 낫다

<br>

## Try-Catch-Finally 문부터 작성하라

- 예외에서 프로그램 안에다 범위를 정의한다는 사실
- 어떤 면에서 try 블록은 트랜잭션과 비슷
- 단위 테스트에서의 리팩터링이 가능
- **강제로 예외를 일으키는 테스트 케이스를 작성**한 후 테스트를 통과하게 코드를 작성하는 방법을 권장

#### TDD (Test-Driven Development)

- 개발보다 테스트를 먼저 작성하는 방식의 개발 방법론
- 핵심 흐름
  - **Red** (실패하는 테스트 작성)
    - 기능을 만들기 전에 테스트 코드를 작성 (기능이 없으니 실패)
  - **Green** (테스트를 통과하는 최소한의 코드 작성)
    - 코드의 퀄리티보다 일단 테스트를 통과하는 것이 중요
  - **Refactor** (리팩토링)
    - 코드 구조나 중복을 정리하면서 개선

<br>

## **미확인**(unchecked) 예외를 사용하라

- 안정적인 소프트웨어를 제작하는 요소로 **확인된 예외가 반드시 필요하지 않다**는 사실
- C#, C++, Python 등은 확인된 예외를 지원하지 않음에도 안정적인 소프트웨어를 구현하기에 무리가 없음
- 확인된 예외는 **OCP**(Open Closed Principle)를 위반
- 확인된 예외는 하위 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 전부 고쳐야 한다..
- 이는 캡슐화를 위반함

<br>

## 예외에 의미를 제공하라

- 자바에서는 모든 예외에 호출 스택을 제공하나 실패 코드의 의도를 파악하려면 **오류 메시지에 정보를 담아** 예외와 함께 던져줘야 함


<br>

## 호출자를 고려해 예외 클래스를 정의하라

- 프로그래머가 오류를 정의할 때 가장 중요한 관심사는 **오류를 잡아내는 방법**
- 외부 API를 사용할 때 감싸기 기법이 최선
  - 외부 API를 감싸면 외부 라이브러리와 프로그램 사이에서 **의존성이 크게 줄어든다**
  - 다른 라이브러리로 갈아타도 비용이 적음
  - **테스트에도 용이**
  - 특정 업체가 API를 설계한 방식이 발목 잡히지 않음

```java
public class LocalPort {
    private ACMEPort innerPort;

    public LocalPort(int portNumber) {
        innerPort = new ACMEPort(portNumber);
    }

    public void open() {
        try {
            innerPort.open();
        } catch (DeviceResponseException e) {
            throw new PortDeviceFailure(e);
        } catch (ATM1212UnlockedException e) {
            throw new PortDeviceFailure(e);
        } catch (GMXError e) {
            throw new PortDeviceFailure(e);
        }
    }
    ...
}
```

<br>

## 정상 흐름을 정의하라

- **특수 사례 패턴** (Special Case Pattern)
  - 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식
  - 클라이언트 코드가 예외적인 상황을 처리할 필요 X

<br>

## **null**을 반환하지 마라

- null을 반환하는 코드는 일거리만 늘리고 호출자에게 문제를 떠넘긴다
- 메서드에서 null을 반환하고픈 유혹이 든다면 그 대신에 **예외를 던지거나 특수 사례 객체를 반환**

```java
List<Employee> employees = getEmployees();
if (employees != null) {
    for(Employees e : employees) {
        totalPay += e.getPay();
    }
}
```

⬇️⬇️⬇️

```java
List<Employee> employees = getEmployees();
for(Employees e : employees) {
    total += e.getPay();
}
```

⬇️⬇️⬇️

```java
public List<Employee> getEmployees() {
    if ( .. 직원이 없다면 ..) 
        return Collections.emptyList();
}
```

> 바로 위 코드처럼 변경한다면 코드도 깔끔해지고 NullPointerException이 발생할 가능성이 줄어든다.

<br>

## **null**을 전달하지 마라

- 정상적인 인수로 null을 기대하는 API가 아니라면 메서드로 null을 전달하는 코드는 최대한 피한다
- 해결 방법
  - **새로운 예외 유형을 만들어**서 던지는 방법
  - **assert** 문을 사용하는 방법

#### assert 문

- 조건이 참인지 검사하는 데 사용되는 디버깅용 도구

```java
assert 조건 : 실패했을 경우 보여줄 메시지;
```

<br>

## 결론

- 깨끗한 코드는 읽기도 좋아야 하지만 **안정성도 높아야** 함
- 오류 처리를 프로그램 논리와 분리하면 **독립적인 추론이 가능**해지며 코드 유지보수성도 크게 높아짐

<br>

## 정리

| 항목 | 핵심 요지 |
|-|-|
| 에외 사용 권장 | 오류 코드보다 예외를 던지는 방식이 로직과 오류 처리를 분리할 수 있어 코드가 더 깔끔하고 명확해짐 |
| Try-Catch-Finally 구조 중심 개발 | 예외 처리 흐름을 중심으로 코드를 구성하면 리팩토링이 쉬워지고 테스트가 용이해짐. 트랜잭션처럼 명확한 경계가 생김 |
| Unchecked 예외 선호 | 자바의 checked 예외는 유연성을 떨어뜨리고 캡슐화를 해침. 대부분의 현대 언어는 unchecked 방식 사용 |
| 의미 있는 예외 메시지 | 단순 스택 트레이스가 아닌 문제를 파악하기 쉬운 정보를 예외에 담아야 함 |
| 예외 감싸기 (Wrapper) | 외부 라이브러리 예외를 직접 노출하지 말고, 자체 예외로 감싸서 추상화하면 테스트와 변경에 유리 |
| 정상 흐름 유지 | 예외 대신 특수 객체로 비정상 상황을 처리하면, 호출자 코드가 간결하고 명확해짐 |
| null 사용 지양 | null 반환/전달은 예외 발생 가능성을 높임. 가능한 한 예외나 빈 컬렉션 등 대체값 사용 권장 |
| 방어적 코드 작성 | 예상치 못한 null이나 잘못된 인수가 들어오지 않도록 assert나 명시적 예외로 방어 로직 추가 |

