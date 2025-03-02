![](https://velog.velcdn.com/images/codegod/post/fa7ece74-ef13-4c7a-9c3d-0d032750ec37/image.png)

### 함수

- 작게 만들어라
- 명령과 수행을 분리하라
- 오류처리도 하나의 작업이다.

2장에서 설명하던 의미있는 이름이 중요한 이유가 여기서 추가 설명된다. if 문 안에 잘지어진 이름의 메소드만 호출되면 수십줄 하던 코드가 단 몇 줄 만에 끝나게 된다.
![](https://velog.velcdn.com/images/codegod/post/a3d59d4f-8b17-4167-a44c-9cb0e41ddbcd/image.png)
![](https://velog.velcdn.com/images/codegod/post/2ac4eb76-4c1c-4913-912d-8611f0347c64/image.png)
이렇게 기괴하던 코드가 다음과 같이 우아한 코드가 된다.
![](https://velog.velcdn.com/images/codegod/post/1385a47a-3735-40a1-9ab6-47142f91030a/image.png)

함수의 들여쓰기가 1단이나 2단에서 끝날 수 있게 쪼개서 작성하면 더 깔끔한 코드가 된다.

명령과 수행을 분리하라는 키워드로 첫 1장에서 설명하던 한가지 기능만 수행하게 함수를 작성하라는 말을 추가 설명한다. 처음에는 당연히 기능을 줄이면 읽기 쉬운 코드가 되지라고 가볍게 생각하고 있었다. 허나 다음과 같은 예시를 보고 분리의 중요성을 깨닫게 되었다.
![](https://velog.velcdn.com/images/codegod/post/01eb9d32-7968-42d7-b7b2-ebf7b0e384b7/image.png)
![](https://velog.velcdn.com/images/codegod/post/fff17433-c20e-45f9-8c00-3416e4abb96f/image.png)
물론 메소드명이 깔끔하게 작성되지 못하여 모호함을 일으킬 수도 있으나, if문 안에 들어간 set은 해당 동작을 이해하는데 헷갈림을 유발한다. ![](https://velog.velcdn.com/images/codegod/post/2f0b1f49-9670-4447-906f-0ec89a6af326/image.png)
이를 위와 같이, username을 설정하고 username의 존재를 확인하는 명령과 수행으로 나눈다면 훨씬 가독성이 뛰어난 코드가 완성된다.

오류처리에 대해서는 하나의 함수로 try-catch문을 분리하여 작성하면 훨씬 깔끔한 코드가 된다. try-catch문을 분리할 생각을 안했었는데 훨씬 가독성이 좋아지는 거 같다. 앞으로는 오류처리 메소드를 나눠서 작성해봐야겠다.

<hr/>