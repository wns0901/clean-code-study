# 2장 의미 있는 이름

## 의도를 분명히 밝혀라

- 변수 이름의 역할
  - 존재 이유
  - 수행 기능
  - 사용 방법

주석이 따로 필요하다면 변수 이름이 의도를 분명히 드러내기 못했다는 말

<br/>

```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>;

  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x);

  return list1;
}
```

```java
public List<int []> getFlaggedCells() {
  List<int []> flaggedCells = new ArrayList<int []>();

  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE] = FLAGGED)
    flaggedCells.add(cell);

  return flaggedCells;
}
```

변수의 이름에 의미만 부여해주어도 코드를 이해하는데 큰 도움이 됨

## 그릇된 정보를 피하라

- 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용하면 안됨

  - 실제 list가 아니면 변수명에 list를 사용하는 것은 좋지 않음

- 흡가한 이름을 사용하지 않도록 주의

- 유사한 개념은 유사한 표기법 사용

## 의미 있게 구분하라

컴파일러나 인터프리터만 통과하려는 생각으로 코드를 구현하는 것은 좋지 않다.

- 연속된 숫자 덧붙이기 X
- 불용어 추가 X
  - Info, Data 역기 의미가 불분명한 불용어
- 읽는 사람이 차이를 알도록 이름을 지어야함

## 발음하기 쉬운 이름을 사용하라

프로그래밍은 사회 활동이기 때문에 발음하기 쉬운 이름이 중요하다.

## 검색하기 쉬운 이름을 사용하라

- e, 숫자등 검색에 어려움이 있는 변수명은 피해야 한다.

## 인코딩을 피하라

### 헝가리식 표기법

> 헝가리식 표기법 : 프로그래밍 언어에서 변수 및 함수의 인자 이름 앞에 데이터 타입을 명시하는 코딩 규칙이다.

옛날에는 컴파일러가 타입을 점검하지 않았으므로 프로그래머에게 타입을 기억할 단서가 필요했지만 지금은 이럴 필요가 없다.

## 클래스 이름

- 명사나 명사구가 적합
- 동사 사용 X

## 메서드 이름

- 동사나 동사구 적합

  - 접근자: get
  - 변경자: set
  - 조건자: is

- 생성자를 오버로딩할 때는 정적 팩토리 메서드 사용
  - 인수를 설명하는 이름 사용

```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);

Comples fulcrumPoint = new Complex(23.0);
```

## 기발한 이름은 피하라

## 한 개념에 한 단어를 사용하라

ex)  
DeviceManager, ProtocolController -> DeviceController, ProtocolController

## 말장난을 하지 마라

한 개념에 한 단어라는 규칙 때문에 여러 클래스에 add라는 메서드가 생겼지만 매갸변수와 반환값이 의미적으로 같으면 상관 없다. 하지만 다르다면 이는 맥락적으로 다르기 때문에 add 대신 insert, append와 같은 다른 이름을 사용하는 것이 좋다.

## 해법 영역애서 가져온 이름을 사용하라

코드를 읽을 사람도 프로그래머이기 떄문에 다음과 같은 이름 사용은 좋은 방법이다.

- 전산 용어
- 알고리즘 이름
- 패턴 이름
- 수학 용어

기술 개념에는 기술 이름이 가장 적합한 선택이다.

## 문제 영역에서 가져온 이름을 사용하라

적절한 프로그래머 용어가 없다면 문제 영역(도메인) 이름을 가져온다

## 의미 있는 맥락을 추가하라

firstName, lastName, street, houseNumber, city, state, zipcode과 같은 이름은 같이 보면 주소라는 것을 금방 알 수 있지만 state 하나만 보면 주소를 뜻한다는 것은 알기 힘들다.

addr라는 접두어를 추가하면 변수가 좀 더 큰 구조에 속하다는 것을 보다 알기 쉬워진다. 물론 Address라는 클래스를 생성하는 것도 좋다.

```java
private void printGuessStatistics(char candidate, int count) {
  String number;
  String verb;
  String pluralModifier;
  if (count = 0) {
    number = "no";
    verb = "are";
    pluralModifier = "s";
  } else if (count = 1) {
    number = "1";
    verb = "is";
    pluraModifier = "";
  } else {
    number = Integer.toString(count);
    verb = "are";
    pluralModifier = "s";
  }

  String guessMessage = String. format(
    "There %s %s %s%s", verb, number, candidate, pluralModifier
  );

  print(guessMessage);

}
```

위에 코드의 경우는 guessMessage메서드가 정의된 부분을 보기 전까지는 number, verb,pluralModifier가 무슨 역할을 하는지 알기 어렵다.
GuessStatisticsMessage라는 클래스를 만든 후 세 변수를 클래스에 넣으면 변수의 맥락이 분명해진다.

```java
public class GuessStatisticsMessage {
  private String number;
  private String verb;
  private String pluralModifier;

  public String make(char candidate, int count) {
  createPluralDependentMessageParts(count);
  return String. format(
    "There %s %s %s%s",
    verb, number, candidate, pluralModifier );
  }
}

private void createPluralDependentMessageParts(int count) {
  if (count = 0) {
    thereAreNoLetters();
  } else if (count = 1) {
    thereIsOneLetter();
  } else {
    thereAreManyLetters(count);
  }
}

private void thereAreManyLetters(int count) {
  number = Integer.toString(count);
  verb = "are";
  pluralModifier = "s";
}

private void thereIsOneLetter() {
  number = "1";
  verb = "is";
  pluralModifier = "";
}

private void thereAreNoLetters() {
  number = "no";
  verb = "are";
  pluralModifier = "s";
}
```

## 불필요한 맥락을 없애라

예를 들어 고급 휘발류 충전소라(Gas Station Deluxe)라는 애플리케이션을 짠다고 했을 때 모든 클래스 이름에 GSD를 넣는 것은 좋지 않다.

이는 IDE에서 자동 완성과 검색에 비효율적이다.

## 마치면서

변수 이름을 쓸 때 발음하기 쉽고 한 번에 알아보기 쉬운게 좋다고 생각하기에 한국인이 우리는 다음 프로젝트할 때
한글 변수를 사용하는 방법도 생각해보면 좋을 거 같다.

이때 한글 변수에 대한 컨벤션은 토스의 세종대왕 프로젝트를 참고해보면 좋을 거 같다.  
<br/>
[세종대왕 프로젝트 (한글 코딩 컨벤션)](https://tosspayments-dev.oopy.io/chapters/frontend/posts/hangul-coding-convention)
