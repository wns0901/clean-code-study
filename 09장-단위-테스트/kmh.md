### 단위 테스트

- TDD 3가지 법칙

    - 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
    - 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
    - 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

- **테스트의 장점** : 유연성, 유지보수성, 재사용성 제공
- 가독성 좋은 테스트 코드를 작성하라
- 테스트당 assert 하나만을 작성하라
- 테스트당 개념 하나만을 작성하라
- **F.I.R.S.T**

이 장에서는 단위 테스트의 중요성을 강조하며 가독성이 좋은 테스트 코드를 작성해야함을 주장한다. 추가적으로 테스트당 assert 하나 만을 사용하라는 학파에 대한 언급과 개념 하나만을 작성하라고 주장한다. 개념 하나 만을 작성하는 것은 가독성적인 면에서 도움이 크게 될 것이라 생각했지만 assert에 대해서는 크게 와닿지 않았다. 아무래도 직접 활용하여 장단점을 비교해봐야겠다.
예시 테스트 코드를 설명하며 **BUILD-OPERATE-CHECK** 패턴에 대한 언급과 코드 패턴에 맞는 테스트 코드의 가독성을 보여준다.

**BUILD-OPERATE-CHECK 패턴**

**패턴의 주요 단계**

**Build (구축)**

- 시스템이나 프로세스를 설계하고 구축

- 요구사항 분석 및 목표 설정을 통해 초기 단계에서 완성물을 구현

**Operate (운영)**

- 구축된 시스템을 운영하면서 실제 데이터를 수집

- 운영 상황에서의 문제나 개선점을 파악

**Check (검토)**

- 운영 결과를 평가하고 성과를 측정

- 수집된 데이터를 분석하여 잘 작동하는 부분과 개선이 필요한 부분을 확인

```java
class SystemProcess {
    private String state;

    public SystemProcess() {
        this.state = "Initial";
    }

    public void build() {
        state = "Build Complete";
        System.out.println("📦 Building the system... Current state: " + state);
    }

    public void operate() {
        System.out.println("⚙️ Operating the system... Current state: " + state);
    }

    public void check() {
        System.out.println("✅ Checking the system...");
        if ("Build Complete".equals(state)) {
            System.out.println("System is running well! No issues found.");
        } else {
            System.out.println("Improvement needed. Returning to the Build phase.");
            build();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        SystemProcess system = new SystemProcess();

        // Build-Operate-Check 반복
        for (int i = 1; i <= 3; i++) {
            System.out.println("\nIteration " + i);
            system.build();
            system.operate();
            system.check();
        }
    }
}


// 실행 결과
Iteration 1
📦 Building the system... Current state: Build Complete
⚙️ Operating the system... Current state: Build Complete
✅ Checking the system...
System is running well! No issues found.

Iteration 2
📦 Building the system... Current state: Build Complete
⚙️ Operating the system... Current state: Build Complete
✅ Checking the system...
System is running well! No issues found.

Iteration 3
📦 Building the system... Current state: Build Complete
⚙️ Operating the system... Current state: Build Complete
✅ Checking the system...
System is running well! No issues found.
```

**F.I.R.S.T**

- **F**ast(빠르게) : 테스트는 빨라야한다.

- **I**dependent(독립적으로) : 각 테스트는 서로 의존하면 안된다.

- **R**epeatable(반복가능하게) : 테스트는 어떤 환경에서도 반복 가능해야 한다.

- **S**elf-Validating(자가검증) : 테스트는 성공 혹은 실패로 결과를 내야한다.

- **T**imely(적시에) : 테스트는 테스트하려는 실제 코드 구현 직전, 적시에 작성해야 한다.