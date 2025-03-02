# 3장 함수

## 작게 만들어라

### 블록과 들여쓰기

- if문 else문 while문 등에 들어가는 블록은 한 줄이어야 한다.
- 들여쓰기 수준은 1단이나 2단을 넘어서면 안된다.

## 한 가지만 해라

- 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한가지 작업만 해야한다.
- 의미 있는 이름으로 다른 함수를 추출할 수 있다면 그 함수는 여러 작업을 하는 셈

## 함수 당 추상화 수준은 하나로

- 함수 내 모든 문장의 추상화 수준이 동일

- 추상화 수준
  - 높음: `getHtml()`
  - 중간: `String pagePathName = PathParser.reder(pagepath);`
  - 낮음: `.append("\n")`

### 위에서 아래로 코드 읽기: 내려가기 규치

- 코드는 위에서 아래로 이야기처럼 읽혀야 좋음.
- 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아짐 -> 내려가기 규칙

### 논의할 부분

[추상화 수준을 하나로의 반박 레퍼런스](https://youtu.be/th7n1rmlO4I?si=wzD7wrAeEoP3iS6O&t=62) <br>
해당 영상에서 보면 DB에서 너무 과한 정규화가 테이블들의 관계를 너무 복잡하게 만들어 성능을 떨어트리는 것처럼 너무 과한 추상화는 결국 코드를 읽을 때 정의된 메소드로 잦은 이동 때문에 오히려 가독성을 저하한다는 의견을 제시한다. 과연 어떤 것이 논의해보면 좋을 거 같다.

## Swith 문

- swith 문은 작게 만들기 어려움
- 다형적 객체 생성에 사용하면 용이함

```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
  switch (e.type) {
    case COMMISSIONED:
      return calculateCommissionedPay (e);
    case HOURLY:
      return calculateHour lyPay(e);
    case SALARIED:
      return calculateSalariedPay(e);
    default:
      throw new InvalidEmployeeType(e.type);
  }
}
```

위 코드는 하나의 동작만 하지 않으며 수정에도 용의하지 않다.   
해당 스위치문을 아래처럼 바꾸면 하나의 동작만 하면서 수정에도 비교적 쉬워진다.

```java
public abstract class Employee {
	public abstract boolean isPayDay();
	public abstract Money calculate();
	public abstract void deliverPay(Money pay);
}

public class EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) {
		switch(r) {
			case COMMISSIONED : return new CommissionedEmployee(r); 
			case HOURLY : return new HourlyEmployee(r);
			case SALARIED : return new SalariedEmployee(r);
			default : return null;
		}
	}
}
```

## 서술적인 이름을 사용하라

- 길고 서술적인 이름이 길고 서술적인 주석보다 좋다.
- 서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해짐


## 함수 인수
- 이상적인 인수 개수는 0개(무항)
- 인수는 개념을 이해하기 어렵게 만듬
- 테스트 관점에서도 인수는 어려움
  - 인수가 3개가 넘어가면 인수마다 유효한 값으로 모든 조합을 구성해 테스트하기가 상당히 부담스러움

- 출력 인수는 입력 인수보다 이해하기 어려움
  - 인수로 결과를 받으리라 기대하지 않음
  - 출력 인수는도작가 코드를 재차 확인하게 만듬

### 단항 형식
- 질문을 던지는 경우
- 인수를 뭔가로 변환해 결과를 반환하는 경우
  - 출력 인수를 이용하면 혼란을 일으킴
- 이벤트
  - 입력 인수만 존재
  - 이벤트라는 사실이 코드에 명확이 들어나야함

### 플래그 인수
플래그 인수의 존재는 함수가 여러 가지를 처리한다는 뜻을 내포하기 때문에 사용을 피하는게 좋다.

### 이항, 삼항 함수
- 인수로 던져주는 순서로 인한 독자의 주춤을 야기할 수 있음

### 인수 객체
- 인수가 2-3개가 필요하다면 따로 클래스 변수를 선언하는 것도 좋음

### 동사와 키워드
- 함수의 의도나 인수의 순송허 의도를 제대로 표현하려면 좋은 함수 이름은 필수
- 단항 함수 -> 함수와 인수가 동사 명사 쌍을 이루는 게 좋음
- 함수 이름에 키워드를 추가하면 인수 순서를 기억할 필요를 줄여줌

## 부수 효과를 일으키지 마라!
- 시간적인 결합, 순서 종속성 초래
> 시간적인 결합: 코드의 실행 순서가 중요한 관계를 의미합니다. 특정 메서드나 함수가 올바르게 작동하기 위해 다른 메서드나 함수가 먼저 실행되어야 하는 상황을 말합니다.

### ex)
```java
public class UserValidator {
  private Cryptographer cryptographer;
  public boolean checkPassword(String userName, String password) {
    User user = UserGateway. findByName (userName) ;
    if (user != User.NULL) {
      String codedPhrase = user.getPhraseEncodedByPassword();
      String phrase = cryptographer.decrypt (codedPhrase, password);

      if ("Valid Password". equals (phrase)) {
        Session.initialize();
        return true;
    }
    return false;
  }
}
```

위 코드에서는 Session.initialize()가 부수효과를 이르킨다. 이는 세션을 초기화하는 코드로 보인다. 그럼으로 잘못 호출하면 의도하지 않게 세션에 정보가 날아간다. 즉 이 메서드는 필연적으로 처음 로그인할 때만 사용되어야 하지만 이름만으로는 이를 유추하기가 어렵다. 때문에 시간적 결합이 필요하다면 함수 이름에 분명히 명시하는 것이 좋다.

## 명령과 조희를 분리하라!

### 함수는 뭔가를 수행하거나 답하거나 둘 중 하나만 해야 함

```java
public boolean set(String attribute, String value);

if (set("username", "unclebob")) ...
```
위 코드는 set 메서드가 set의 의미에 혼동을 줄 수 있기에 다음과 같이 고치는 것이 좋다

```java
if ( attributeExists("username")) {
  setAttribute("username", "unclebob");
  ...
}
```

## 오류 코드보다는 예외를 사용하라!
- 명령 함수에서 오류 코드를 반환하는 것은 명령/조희 분리 규칙을 위반함
```java
if (deletePage (page) = E_OK)
```
- 코드를 쫌 더 깔끔하게 작성 가능
```java
if (deletePage(page) = E_OK) {
  if (registry. deleteReference(page.name) = E_OK) {
    if (configKeys.deleteKey(page.name.makeKey() == E_OK) {
      logger.log ("page deleted");
    } else {
      logger.log ("configkey not deleted");
    }
  } else {
    logger.log("deleteReference from registry failed");
  }
} else {
    logger.log("delete failed");
    return E_ERROR;
}

# --------------------------------------------

try {
  deletePage(page);
  registry.deleteReference(page.name);
  configkeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
  logger.log(e.getMessage());
}
```

- try/cathc 블록을 별도 함수로 뽑아내는 편이 좋음
```java
public void delete(Page page) {
  try {
    deletePageAndAllReferences(page) ;
  }
  catch (Exception e) {
    logError(e);
  }
}

private void deletePageAndAllReferences(Page page) throws Exception {
  deletePage (page) ;
  registry.deleteReference (page.name) ;
  configkeys.deleteKey(page.name.makeKey());
}

private void logError (Exception e) {
  logger. log(e.getMessage());
}
```

## 반복하지 마라

## 구조적 프로그래밍

## 함수를 어떻게 짜죠?
- 글짓기와 같이 먼저 생각을 기록한 후 읽기 좋게 다듬는다.

- 처음에는 일단 작성
- 코드를 다듬기
- 함수 만들기
- 이름 바꾸기
- 중복 제거
- 메서드 줄이고 순서 바꾸기

이 과정을 효율적으로 하기 위해서는 단위 테스트 코드를 짜는 게 좋다.

## 결론
- 함수는 그 프로그래밍 언어에서 동사, 클래스는 명사
- 프로그래머는 시스템을 구현할 플로그램이 아니라 풀어갈 이야기로 여겨야 한다.
