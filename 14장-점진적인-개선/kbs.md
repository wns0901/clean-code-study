# 14장 점진적인 개선
우선 저자가 작성한, 유의깊게 읽을 가치가 있고 이를를 본받을만한 코드의 구성을 보자

~~~java
//사용법
public static void main(String[] args) {
	try {
		Args arg = new Args("l,p#,d*", args);
		boolean logging = arg.getBoolean('l');
		int port = arg.getInt('p');
		String directory = arg.getString('d');
		executeAppliocation(logging, port, directory);
	} catch (ArgsException e) {
		System.out.printf("Argument error: %s\n", e.errorMessage());
	}
}
~~~
args 변수에 질의하는, 아주 단순한 사용법을 보여준다. <br><br>

~~~java
//Args 클래스
import static com.objectmentor.utilities.args.ArgsException.ErrorCode.*;

public class Args {
	private Map<Character, ArgumentMarshaler> marshalers;
	private Set<Character> argsFound;
	private ListIterator<String> currentArgument;

	public Args(String schema, String[] args) throws ArgsException {
		marshalers = new HashMap<Character, ArgumentMarshaler>();
		argsFound = new HashSet<Character>();
    
    parseSchema(schema);
    parseArgumentStrings(Arrays.asList(args)); 
  }
  
  private void parseSchema(String schema) throws ArgsException { 
    for (String element : schema.split(","))
      if (element.length() > 0) 
        parseSchemaElement(element.trim());
  }
  
  private void parseSchemaElement(String element) throws ArgsException { 
    char elementId = element.charAt(0);
    String elementTail = element.substring(1); validateSchemaElementId(elementId);
    if (elementTail.length() == 0)
      marshalers.put(elementId, new BooleanArgumentMarshaler());
    else if (elementTail.equals("*")) 
      marshalers.put(elementId, new StringArgumentMarshaler());
    else if (elementTail.equals("#"))
      marshalers.put(elementId, new IntegerArgumentMarshaler());
    else if (elementTail.equals("##")) 
      marshalers.put(elementId, new DoubleArgumentMarshaler());
else if (elementTail.equals("[*]"))
      marshalers.put(elementId, new StringArrayArgumentMarshaler());
    else
      throw new ArgsException(INVALID_ARGUMENT_FORMAT, elementId, elementTail);
  }
  
  private void validateSchemaElementId(char elementId) throws ArgsException { 
    if (!Character.isLetter(elementId))
      throw new ArgsException(INVALID_ARGUMENT_NAME, elementId, null); 
  }
  
  private void parseArgumentStrings(List<String> argsList) throws ArgsException {
    for (currentArgument = argsList.listIterator(); currentArgument.hasNext();) {
      String argString = currentArgument.next(); 
      if (argString.startsWith("-")) {
        parseArgumentCharacters(argString.substring(1)); 
      } else {
        currentArgument.previous();
        break; 
      }
    } 
  }
  
  private void parseArgumentCharacters(String argChars) throws ArgsException { 
    for (int i = 0; i < argChars.length(); i++)
      parseArgumentCharacter(argChars.charAt(i)); 
  }
  
  private void parseArgumentCharacter(char argChar) throws ArgsException { 
    ArgumentMarshaler m = marshalers.get(argChar);
    if (m == null) {
      throw new ArgsException(UNEXPECTED_ARGUMENT, argChar, null); 
    } else {
      argsFound.add(argChar); 
      try {
        m.set(currentArgument); 
      } catch (ArgsException e) {
        e.setErrorArgumentId(argChar);
        throw e; 
      }
    } 
  }
  
  public boolean has(char arg) { 
    return argsFound.contains(arg);
  }
  
  public int nextArgument() {
    return currentArgument.nextIndex();
  }
  
  public boolean getBoolean(char arg) {
    return BooleanArgumentMarshaler.getValue(marshalers.get(arg));
  }
  
  public String getString(char arg) {
    return StringArgumentMarshaler.getValue(marshalers.get(arg));
  }
  
  public int getInt(char arg) {
    return IntegerArgumentMarshaler.getValue(marshalers.get(arg));
  }
  
  public double getDouble(char arg) {
    return DoubleArgumentMarshaler.getValue(marshalers.get(arg));
  }
  
  public String[] getStringArray(char arg) {
    return StringArrayArgumentMarshaler.getValue(marshalers.get(arg));
  } 
}
~~~
생성자를 통해 매개변수를 받고, 생성자에 전체적인 흐름을 정의한다.<br>
메소드는 어떻게 사용하는지를 먼저 기재한 뒤 정의한다.<br>
이를 통해 위에서 아래로 읽듯이 코드 리딩이 가능하고, 또한 가장 큰 개념을 다루는 메소드를 위에, 작은 개념을 다루는 메소드를 나중에 기재함으로서 가독성을 높혔다.<br>

~~~java
public interface ArgumentMarshaler {
  void set(Iterator<String> currentArgument) throws ArgsException;
}
~~~
~~~java
public class BooleanArgumentMarshaler implements ArgumentMarshaler { 
  private boolean booleanValue = false;
  
  public void set(Iterator<String> currentArgument) throws ArgsException { 
    booleanValue = true;
  }
  
  public static boolean getValue(ArgumentMarshaler am) {
    if (am != null && am instanceof BooleanArgumentMarshaler)
      return ((BooleanArgumentMarshaler) am).booleanValue; 
    else
      return false; 
  }
}

public class StringArgumentMarshaler implements ArgumentMarshaler { 
  private String stringValue = "";
  
  public void set(Iterator<String> currentArgument) throws ArgsException { 
    try {
      stringValue = currentArgument.next(); 
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_STRING); 
    }
  }
  
  public static String getValue(ArgumentMarshaler am) {
    if (am != null && am instanceof StringArgumentMarshaler)
      return ((StringArgumentMarshaler) am).stringValue; 
    else
      return ""; 
  }
}

public class IntegerArgumentMarshaler implements ArgumentMarshaler { 
  private int intValue = 0;
  
  public void set(Iterator<String> currentArgument) throws ArgsException { 
    String parameter = null;
    try {
      parameter = currentArgument.next();
      intValue = Integer.parseInt(parameter);
    } catch (NoSuchElementException e) {
      throw new ArgsException(MISSING_INTEGER);
    } catch (NumberFormatException e) {
      throw new ArgsException(INVALID_INTEGER, parameter); 
    }
  }
  
  public static int getValue(ArgumentMarshaler am) {
    if (am != null && am instanceof IntegerArgumentMarshaler)
      return ((IntegerArgumentMarshaler) am).intValue; 
    else
    return 0; 
  }
}
~~~

ArgumentMarshaler를 정의하는 부분이다.<br>
모든 메소드에서 공통으로 사용하는 코드(이 코드에선 셋)를 인터페이스로 정의하고, 타입 별로(이 예시에선 boolen, string, int만 다룬다) 셋을 재정의하고 겟 메소드를 추가한다.<br><br>

따라서 새로운 타입을 원한다면 코드 수정은 간단하다.<br>
ArgumentMarshaler에 타입에 맞는 적절한 메소드를 추가하고, Args.parseSchemaElement에 if-else 구문 한 줄을 추가한다.<br>
실로 간단하며, 유지보수에 강한 자바의 강점을 살린 코드라고 이야기한다.<br>

프로그래밍은 과학보다 공예에 가깝다고 표현.<br>
우선 기능만은 하도록, 즉 단위 테스트를 통과하는 코드 덩어리를 작성한 뒤, 수번 십수번 수십번 수정하여 코드를 정리한다.<br><br>

저자가 작성한 1차 초안은 더럽다.<br>
하나의 파일에 모든 코드가 담겨있다.<br>
어렴풋이 계층이다 싶은 무언가는 보이지만, 명확히 알 수 없고 이해가 힘들다.<br>
더 정확히는 몇 줄 읽어보니 그냥 읽기 싫어지는 코드다.<br>

이 코드는 처음부터 나온 것은 아니며, boolen만 다루던 코드에서 문자열과 정수를 다루기 시작하며 가독성이 망가졌다 한다.<br>

boolen만 다루는 코드는 작다.<br>
매우 작아서 굳이 분리할 필요성도 느끼지 못할만하다.<br>

## 리팩토링의 과정 - 점진적인 개선

### 개선의 주의점 - 바닥부터 갈아엎기. 오히려 이전과 같이 돌아가는 코드를 만들기 난해.
- TDD를 따라 junit과 fitness를 통해 각각 단위 테스트 슈트와 인수 테스트를 작성.
- 클래스에서 분리/세분화할 수 있는 개념 탐색(리팩토링의 목표) - 예시 코드에선 ArgumentMashaler
- 분리한 개념의 틀을 작성하고, 분리된 클래스를 사용.
- 그럼으로서 변경해야하는 부분 확인 - 예시 코드에선 parse, get, set
- 이때, 인수 타입 변경과 관련된 경우 파생 클래스를 우선하기 보다 일단 단일 클래스에 몰아넣은 후, 나머지가 완성되면 수정하는 편이 쉽다고 함.
- 새로운 클래스와 메소드를 만듦으로서, 사용하지 않는 기존 메소드 삭제

**이 과정을 반복한다.**<br>
당연하게도 이 과정에서 모든 단위 테스트를 통과해야한다.<br>
이때 사용처가 적어진(그러나 여전히 사용되는) 메소드는 인라인으로 변경하는 것도 고려한다.<br><br>

예외처리 등도 외부로 분리한다. 예외에 관한 사항을 오직 예외만 처리하는 클래스로 옮기는 것만으로 코드가 훨씬 깔끔해진다.


## 그저 돌아가는 코드는 한참 부족하다.
돌아가는 코드는 쉽게 망가진다.<br>
나쁜 일정은 새로 짜면 된다.<br>
나쁜 요구사항은 다시 정의하면 된다.<br>
나쁜 팀 역학은 복구하면 된다.<br>
그러나 나쁜 코드는 **썩어 문드러진다.**<br>
개선은 물론 가능하다. 막대한 시간과 자원과 노력을 투자한다면.<br>
그러나 처음부터 깨끗한 - 구조가 올바르게 짜여져 있고, 변수/클래스명이 합리적인 - 코드를 유지하긴 쉽다.