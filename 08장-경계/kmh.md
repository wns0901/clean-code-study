### 경계

- Map이나 유사한 경계 인터페이스를 불필요하게 넘기지 마라
- 학습 테스트
- log4j

8장에서는 외부 코드를 자신의 코드에 적용할 때의 경계선에 대한 내용을 설명한다. 과거 나도 라이브러리 코드를 커스터마이징했던 경험이 있는데 이 장에서 얻은 지식을 미리 알고 있었더라면 훨씬 더 수월했을 것이라 생각한다. 외부 코드를 사용할 때 최대한 편리하게 사용하기 위한 방법으로 프로그램에 필요한 인수나 반환값만을 사용하는 방법을 언급하며 이를 위해서는 테스트 기반 코딩이 중요함을 강조한다.

```java
// log4j 패키지를 사용하여 로깅 기능 구현 예시
@Test
public void testLogCreate() {
	Logger logger = Logger.getLogger("MyLogger");
    logger.info("hello");
  }
  // 위의 테스트 케이스 가동 시, Appendder라는 무엇인가가 필요하다는 오류가 발생함.
  
// 문서 상 ConsoleAppender라는 클래스가 존재하여 이를 생성 후 재가동
@Test
public void testLogAddAppender() {
	Logger logger = Logger.getLogger("MyLogger");
    ConsoleAppender appender = new ConsonleAppender();
    logger.addAppender(appender);
    logger.info("hello");
  }
  
// 테스트 과정에서 Appender 출력 스트림 없다는 사실 발견
@Test
public void testLogAddAppender() {
	Logger logger = Logger.getLogger("MyLogger");
    logger.removeAllAppenders();
    logger.addAppender(new ConsoleAppender(
    	new PatternLayout("%p %t %m%n"),
        ConsoleAppender.SYSTEM_OUT));
   	logger.info("hello");
  }
  
// 여러 번의 테스트 결과 설정되지 않은 생성자, 인수 문제 확인 후 단위 테스트
public class LogTest {
	private Logger logger;
    
    @Before
    public void initialize() {
    	logger = Logger.getLogger("logger");
        logger.removeAllAppenders();
        	Logger.getRootLogger().removeAllApenders();
          }
          
          @Test
          public void basicLogger() {
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

위의 예시처럼 테스트 주도 개발을 하면 외부 코드를 자신의 코드에 맞게 수정하기 용이하다. 즉, 외부 코드와의 경계를 깨끗하게 만드는 데 가장 중요한 역할을 한다.
