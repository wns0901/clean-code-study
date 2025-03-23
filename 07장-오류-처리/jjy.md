# 7장 오류 처리
- 오류 처리 코드로 인해 프로그램 논리를 이해하기 어려워진다면 깨끗한 코드라 부르기 어려움

## 오류 코드보다 예외를 사용하라

## Try-Catch-Finally 문부터 작성하라

- 예외가 발생할 코드를 짤 때는 Try-Catch-Finally 문으로 시작하는 편이 좋음 <br>
  -> try 블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기 쉬워짐

- 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법 권장 <br>
  -> 자연스럽게 try 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지가 쉬워짐

## 미확인(unchecked) 예외를 사용하라
- 확인된 예외는 OCP를 위반
  - 하위 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 전부 고쳐야함
  - throws 경로에 위치하는 모든 함수가 최하위 함수에서 던지는 예외를 알아야 하므로 캡슐화 깨짐

## 예외에 의미를 제공하라
- 예외를 던질 때 전후 상황을 충분히 덧붙이기 <br>
  -> 오류 발생 원인과 위치 찾기 쉬워짐

- 오류 메시지에 정보를 담아 예외와 함께 던지는게 좋음

- 실패한 연산 이름과 실패 유형도 언급하는게 좋음

## 호출자를 고려해 예외 클래스를 정의

- 예외를 잡아 변환하는 감싸기 클래스를 사용

## 정상 흐름을 정의
- 특수 사례 패턴(Special Case Pattern)을 사용하여 클라이언트 코드가 예외적인 상활을 처리할 필요없게 하라
```java
try {
  MealExpenses expenses = expenseReportDAO. getMeals (employee.get ID());
  m_total += expenses.getTotal();
} catch (MealExpensesNotFound e) {
  m_total += getMealPerDiem);
}

// --------- 수정 ---------

MealExpenses expenses = expenseReportDA0.getMeals (employee.getID());
m_total += expenses.gettotal();

public class PerDiemMealExpenses implements MealExpenses {
  public int getTotal {
  // 기본값으로 일 기본 식비를 반환한다.
  }
}
```

## null을 반환하지 마라
- null을 반환할 경우
  - 하나라도 null 확인을 빼먹는 다면 통제 불능에 빠지기 쉬움

- null을 반환하고 싶은 유혹이 들면 그 대신 예외를 던지거나 특수 사례 객체를 반환이 좋음
- 사용하는 외부 API가 null을 반환한다면 감싸기 메서드를 구현해 예외를 던지거나 특수 사례 객체를 반환하는 방식을 고려

## null을 전달하지 마라
- 대다수 프로그래밍 언어는 호출자가 실수로 넘기는 null을 적절히 처리하는 방법이 없음 <br>
  -> 애초에 null을 넘기지 못하도록 금지하는 정책이 합리적