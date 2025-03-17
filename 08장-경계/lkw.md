# 이경원

## 경계

---

## 소프트웨어 경계를 깔끔하게 처리하는 기법과 기교를 살펴볼 예정

<br>

## 외부 코드 사용하기

- 외부 코드의 예시로 java.util.Map이 있다
  - Map은 굉장히 다양한 인터페이스로 수많은 기능을 제공하나 그만큼 위험도 크다
  - Map이 반환하는 Object를 올바른 유형으로 변환할 책임은 Map을 사용하는 클라이언트에 있음
  - 대신 제너릭스(Generics)을 사용하면 코드 가독성이 크게 높아짐
  - 경계 인터페이스인 Map을 Sensor 안으로 숨긴다
  - Map 인터페이스가 변하더라도 나머지 프로그램에는 영향을 미치지 않음음

```java
Map sensors = new HashMap();

Sensor s = (Sensor)sensors.get(sensorId);
```

⬇️⬇️⬇️

```java
// 제네릭스 사용
Map<String, Sensor> sensors = new HashMap<Sensor>();

Sensor s= sensors.get(sensorId);
```

⬇️⬇️⬇️

```java
// Sensors 클래스 안에서 객체 유형을 관리
public class Sensors {
    private Map sensors = new HashMap();

    public Sensor getById(String id) {
        return (Sensor) sensors.get(id);
    }
}
```

> Map 클래스를 사용할 때마다 위와 같이 캡슐화하라는 뜻이 아니라 Map을 여기저기에 넘기지 말아야 함


<br>

## 경계 살피고 익히기

- 외부 코드를 익히고 통합함은 굉장히 어려움
- 그래서 **학습 테스트**라는 용어가 등장
  - 루이쪽 코들르 작성해 외부 코드를 호출하는 대신 먼저 **간단한 테스트 케이스를 작성**해 외부 코드를 익힘
  - 통제된 환경에서 API를 제대로 이해하는지를 확인하는 셈

<br>

## log4j 익히기

- 로깅 레벨 지원 (`TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`)
- 디양힌 출력 대상
- 성능 최적화 (비동기 로깅, 필터링 등)

```java
public class LogTest {
    private Logger logger;

    @Before
    public void initialize() {
        logger = Logger.getLogger("logger");
        logger.removeAllAppenders();
        Logger.getRootLogger().removeAllAppenders();
    }

    @Test
    pubilc void basicLogger() {
        BasicConfigurator.configure();
        logger.info("basicLogger");
    }

    @Test
    public void addAppenderWithStream() {
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n"),
            ConsoleAppender.SYSTEM_OUT));
        logger.info("addAppenderWithStream");
    }

    @Test
    public void addAppenderWithoutStream() {
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n")));
        logger.info("addAppenderWithoutStream");
    }
}
```

<br>

## **학습 테스트**는 공짜 이상이다

- 학습 테스트는 **이해도를 높여주는 정확한 실험**
- 투자하는 노력보다 얻는 성과가 큼

<br>

## 아직 존재하지 않는 코드를 사용하기

- 아는 코드와 모르는 **코드를 분리**하는 경계
- 모르는 코드와 직접 연결되면 유지보수와 테스트가 어려움
- 그래서 이를 해결하기 위해 **우리만의 인터페이스**를 정의하는 것이 좋다

#### 장점

- **코드 분리** → API가 바뀌어도 변경 범위를 최소화할 수 있음
- **가독성 & 유지보수성** → 우리 코드가 외부 API에 의존하지 않도록 깨끗하게 관리됨
- **테스트 가능** → 가짜 구현체를 만들어 독립적인 테스트 가능

<br>

## 깨끗한 경계

- 경계에 위치하는 코드는 깔끔히 분리
- 통제가 불가능한 외부 패키지에 의존하는 대신 통제 가능한 **우리 코드에 의존**하는 편이 훨씬 좋다.
- 새로운 클래스로 경계를 감싸거나 **ADAPTER 패턴**을 사용

<br>

## 정리
| 주제 | 설명 |
|-|-|
| 외부 코드 사용하기 | 외부 라이브러리를 직접 사용하면 불안정함 → 캡슐화 필요 |
| 경계 캡슐화 | 외부 API 변경 시 내부 코드 영향 최소화 |
| 학습 테스트 | 작은 테스트 코드로 외부 API 기능을 익힘 |
| Log4j 테스트 예제 | 기본 설정 및 로깅 기능을 검증하는 코드 작성 |
| 학습 테스트의 이점 | 적은 노력으로 API 이해도 증가, 변경에도 대비 가능 |
| 존재하지 않는 코드 사용 | 예상 인터페이스를 먼저 정의하여 개발 진행 |
| 깨끗한 경계 설계 | Adapter 패턴 사용으로 외부 API 의존성 최소화 |

- 외부 라이브러리는 직접 사용하지 말고 **캡슐화**하자
- **학습 테스트**를 통해 API를 익히고 변경될 가능성에 대비하자
- 아직 없는 API와 연결해야 한다면, **우리만의 인터페이스**를 먼저 설계하자
- **Adapter 패턴**을 활용하면, 외부 API가 변경되더라도 내부 코드 변경을 최소화할 수 있다.