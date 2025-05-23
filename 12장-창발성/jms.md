# 클린 코드 12장: 창발적 설계


## 1) 창발적 설계를 위한 4가지 핵심 규칙 (XP에서 유래)

### 1. 모든 테스트를 통과하라
- 테스트는 신뢰의 기반이자, 리팩터링을 위한 안전망이다.
- 테스트가 잘 짜여 있으면 코드를 과감히 개선할 수 있다.

### 2. 중복을 없애라
- 중복은 나쁜 설계의 신호.
- **비슷한 코드가 보이면 추상화, 함수 추출, 상속 등으로 제거하자.**

### 3. 프로그래머 의도를 표현하라
- 변수명, 함수명, 클래스명으로 **의도를 명확히 전달**하자.
- **코드는 '읽히는 글'이어야 한다.**

### 4. 클래스와 메서드 수를 최소화하라
- 단순한 구조를 유지하는 것이 가장 유지보수하기 쉽다.
- 클래스나 함수가 **역할 이상으로 비대해지지 않도록 관리하자.**


