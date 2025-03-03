# 이경원

## 함수

---

#### 함수를 만드는 규칙 1
- **작게 만들어라**
- 중첩 구조가 생길만큼 함수가 커져서는 안된다

<br>

#### 함수를 만드는 규칙 2
- **한 가지만 해라**
- 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한가지만 수행할 수 있다.
- 우리가 함수를 만드는 이유는 추상화 수준에서 여러 단계로 나누어 수행하기 위해서이다.

<br>

#### 함수를 만드는 규칙 3
- **함수 당 추상화 수준은 하나로**
- 함수가 확실히 하나의 작업만 수행하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.
- 특정 표현이 근본 개념인지 세부사항인지 구분하기 어렵기 때문에 추상화 수준을 섞으면 코드를 읽기 어려워진다.

<br>

#### **내려가기 규칙**
- 위에서 아래로 코드 읽기
- 이야기처럼 읽혀야 좋다.

<br>

#### Switch 문
- switch 문은 한가지 작업만 수행하게 만들기 어렵고 짧게 만들기 어렵다.
- 하지만 다형성을 이용해서 저차원 클래스에 숨기고 반복하지 않게 하는 방법이 있다.

```java
public Money calculatePay(Employee e)
throw InvalidEmployeeType {
    switch (e.type) {
        case COMMISSIONED:
            return calculateCommissionedPay(e);
        case HOURLY:
            return calculateHourly(e);
        case SALARIED:
            return calculateSalariedPay(e);
        default:
            throw new InvalidEmployeeType(e.type);
    }
}
```

- 위 함수의 문제점
  - 함수가 길다.
  - 한 가지 작업만 수행하지 않는다.
  - Single REsponsibility Principle을 위반한다. (코드를 변경할 이유가 여럿이기 떄문)
  - Open Closed Principle을 위반한다
- 해결 방법
  - 추상 팩토리에 숨긴다.
  - 다형성으로 인해 실제 파생 클래스의 함수가 실행된다.

<br>

#### 함수를 만드는 규칙 4
- **서술적인 이름을 사용하라**
- 함수가 작고 단순할수록 서술적인 이름을 고르기도 쉬워진다.
- 길고 서술적인 이름이 짧고 어려운 이름보다 좋다.
- 길고 서술적인 이름이 서술적인 주석보다 좋다.
- 서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해지므로 코드를 개선하기 쉬워진다.
- 이름을 붙일 떄는 일관성이 있어야 한다.

<br>

#### 함수 인수
- 가장 이상적인 인수 개수는 0개이다.
- 인수가 2-3개 필요하다면 일부를 독자적인 클래스 변수로 선언할 가능성을 짚어봐야 한다.
- 함수 이름에 인수의 키워드를 넣는 것이 더 좋고 인수의 순서를 기억할 필요가 없어진다.

<br>

#### 함수를 만드는 규칙 5
- **부수 효과를 일으키지 마라**

```java
public class UserValidator {
    private Cryptogrpher cryptogrpher;

    public boolean checkPassword(String userName, String password) {
        User user = UserGateway.findByName(userName);
        if (user != User.null) {
            String codedPhrase = user.getPhraseEncodedByPassword();
            String phrase = cryptographer.decrypt(codedPhrase, password);
            if ("Valid Password".equals(phrase)) {
                Session.initialize();
                return true;
            }
        }
        return false;
    }
}
```

- 문제점 
  - checkPassword라는 메서드 이름에서 session에 저장된 정보를 초기화한다는 사실을 알 수 없다.
- 해결 방안
  - 함수의 이름을 checkPasswordAndInitializeSession이라는 이름이 더 좋다 
  - 하지만 이는 함수가 한가지만 한다는 규칙을 위반하였다.


<br>

#### 함수를 만드는 규칙 6
- **명령과 조회를 분리하라**
- 함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야 한다.
- 즉, 객체의 상태를 변경하거나 객체 정보를 반환하거나 둘 중 하나.

```java
if (set("username", "unclebob"))...

⬇️⬇️⬇️

if (attributeExists("username")) {
    setAttribute("username", "unclebob");
    ...
}
```

- 위 코드는 set이라는 함수가 username이 unclebob으로 설정되어 있는지 확인하는 코드인지, 설정하는 코드인지 분간하기 어렵다.

<br>

#### 함수를 만드는 규칙 7
- **오류 코드보다 예외를 사용하라**
- 오류 코드 대신에 예외를 사용하면 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다.
- 하지만 try/catch 블록은 코드 구조에 혼란을 일으키며, 정상 동작과 오류 처리 동작을 뒤섞기 때문에 별도 함수를 처리하는 편이 좋다.
- 즉, 정상 동작과 오류 처리 동작을 분히하여 코드를 작성해야 한다.

<br>

#### 함수를 만드는 규칙 8
- **반복하지 마라**
- 중복을 없애면 모듈 가독성이 크게 높아진다.
- 구조적 프로그래밍, AOP(Aspect Oriented Programing), COP(Component Oriented Programing) 어떻게 보면 모두 중복 제거 전략이다.


<br>

#### 구조적 프로그래밍
- 모든 함수 내 모든 블록에 입구와 출구가 하나만 존재해야 한다.
- 함수가 작다면 위 규픽은 별 이익을 제공하지 못할 수 있다.
- 의도를 표현하기 쉽다면..

