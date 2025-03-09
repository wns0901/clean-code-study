![](https://velog.velcdn.com/images/codegod/post/44a1de85-ef0a-4837-a534-613947600909/image.png)
### 자료 추상화

- 구현을 감춰라

```java
public class Point {
	public double x;
    public double y;
}

public interface Point {
	double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```
위의 예시를 보고는 정확히 저자가 요구하는 추상화가 뭔지 이해하기 어려웠다.

```java
public interface Vehicle {
	double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}

public interface Vehicle {
	double getPercentFuelRemaing();
}
```
다음 예시를 보고 저자의 의도를 이해할 수 있었다. 각 동작에 대한 명칭은 개발자가 이해할 수 있게 구성하되, 동작을 외부로 노출시키지 않아 은닉성을 높이라는 것이었다.


### 디미터 법칙

- 객체는 조회 함수로 내부 구조를 공개하면 안된다

클래스 C의 메소드 f에서는 클래스 C, f가 생성한 객체, f 인수로 넘어온 객체, C 인스턴스 변수에 저장된 객체의 메소드만 호출해야 한다고 주장한다.
```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
폴더의 절대경로를 가져기 위한 코드라는 점은 눈에 들어왔으나 정확히 무엇을 설명하는지 이해하기 어려웠다.
```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsoluttePath();
```
기차 충돌을 언급하며 위의 코드로 나누는 편이 좋다고 주장하는데 확실히 조잡함이 줄어드는 점을 알 수 있었다.


### 자료 전달 객체

- DTO
- 활성 레코드

저자는 DTO에 대해 설명하며 bean구조를 사이비 캡슐화라고 주장한다. 또한 활성 레코드에 비즈니스 로직 메소드를 추가하는 개발자에 대하여도 언급하며 자료 구조와 객체도 아닌 잡종 구조가 나온다고 비판한다. 나도 DTO를 작성할 때 데이터 관계도 상의 문제로 인해 비즈니스 로직을 추가했었던 기억이 났다. 과연 최선의 선택이었을까. 해당 코드에 대해서는 리팩토링을 해봐야겠다.