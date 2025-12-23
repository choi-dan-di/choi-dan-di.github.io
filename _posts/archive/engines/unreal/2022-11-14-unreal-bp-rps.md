---
title: "[Unreal Engine] BP - 연습 문제 : 가위바위보"
excerpt: "블루프린트 열거형을 이용해 가위바위보 구현해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, practice, enum, rock paper scissors, rps]

permalink: /unreal/bp-rps/

toc: true
toc_sticky: true

date: 2022-11-14 21:48:40+0900
last_modified_at: 2022-11-14 21:48:42+0900
---

## 👻 가위바위보 구현하기
이번 시간에는 이전 시간에 배웠던 열거형을 응용해 컴퓨터와 하는 가위바위보 게임을 구현해보자.   

그 전에 **플레이어와 컴퓨터의 결과값을 출력**하는 이벤트를 따로 만들어두었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/print-choice.PNG)   

> ``` Add Custom Events ```를 통해 이벤트를 개인적으로 생성할 수 있다.

***

### 🌱 Answer 1
대략적인 알고리즘은 이렇다.

> 1. 1번은 바위, 2번은 보, 3번은 가위를 선택한다.
2. 컴퓨터의 값은 랜덤 함수로 정한다.
3. **무승부 여부**를 먼저 판단한다.
4. 무승부가 아니라면 **승패 여부**를 판단한다.
5. 승패 결과 출력

- **1번은 바위, 2번은 보, 3번은 가위를 선택한다.**
- **컴퓨터의 값은 랜덤 함수로 정한다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/1-1.PNG)   

플레이어는 SET 함수를 사용해 각 번호에 맞는 키와 연결해주고, 컴퓨터는 랜덤 함수를 이용해 0, 1, 2중에 랜덤한 값을 저장하도록 만들었다.

- **무승부 여부를 먼저 판단한다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/1-2.PNG)   

브랜치를 사용해 먼저 두 값이 동일한지 판단했다. 일치하면 바로 무승부를 출력하고 일치하지 않으면 승패를 따지는 알고리즘으로 넘어가도록 만들었다.

- **무승부가 아니라면 승패 여부를 판단한다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/1-3.PNG)   

브랜치의 False 부분을 활용해 승패 여부를 판단했다. 먼저 ``` switch on ```을 사용해 플레이어가 무슨 값을 가지는지 판단했다. 그리고 각 값에 따라 브랜치를 새로 만들어주었다. 동일한 값은 이미 위에서 판단했으니 플레이어의 값이 아닌 컴퓨터의 값만 판별하게끔 조건식을 걸었다.

위에서 보면 플레이어가 바위일 때 컴퓨터는 보, 플레이어가 보 혹은 가위일 때 컴퓨터는 바위를 비교하도록 설정했다. 약간 나만 보기 편하게 만들어둔 것 같아서 마음에 들지는 않지만 노드를 더 쓰고 싶진 않았다... 지금은 가위바위보 세 가지밖에 없어서 이렇게 해도 상관없지만 비교하는 가짓수가 많아지면 확실한 방법으로 만들어야할 것 같다.

- **승패 결과 출력**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/1-4.PNG)   

무승부 제외하고 승패 둘 중에 맞는 결과를 각각 연결시켜주었다.

- **결과**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/result-1.PNG)   

플레이어가 1, 2, 3번 중에 값을 입력하면 승패 결과가 출력된다.

***

### 🌱 Answer 2
2번 답은 초기 설정은 비슷하지만 뒷부분이 약간 다르다.

> 1. 4번은 바위, 5번은 보, 6번은 가위를 선택한다.
2. 컴퓨터의 값은 랜덤 함수로 정한다.
3. 승패 결과를 판단해 결과를 바로 출력한다.

1번 답보다는 조금 더 간단해졌다.

- **4번은 바위, 5번은 보, 6번은 가위를 선택한다.**
- **컴퓨터의 값은 랜덤 함수로 정한다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/2-1.PNG)   

1번 답과 크게 다른 부분이 없다.

- **승패 결과를 판단해 결과를 바로 출력한다.**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/2-2.PNG)   

여기서는 무, 승, 패를 모두 한 번에 비교해서 승패 결과를 출력시켰다. 승패 결과를 판단하는 부분을 ``` Check Winner ```라는 커스텀 이벤트로 묶었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-rps/2-3.PNG)   

``` Check Winner ```의 기능은 이렇다. 플레이어와 컴퓨터 모두 ``` switch on ``` 노드를 사용하여 비교했다.

플레이어 값을 먼저 놔둔다음, 컴퓨터의 값을 각각 비교했다. 1번 답보다 훨씬 간편해지고 규칙적인 답이 완성되었다. 훨씬 확실하고 효율적인 것 같다.

***

## 👻 글을 마치며
이번 시간에는 가위바위보를 구현해보며 블루프린트 활용법을 심화적으로 익혀보았다. 일반 코드로 구현할 때에도 내 코드가 마음에 안 들었는데 블루프린트도 마찬가지였다.. 그래도 한 눈에 보이니까 시간은 훨씬 덜 걸렸던 것 같다. 그래도 그냥 코드를 작성할 때보다 언리얼 에디터에서 구현하는 것들은 실제 게임을 만든다는 가정하에 퍼포먼스까지 생각해야하니까 조금 더 깊은 고민이 필요할 것 같다. 언리얼에서 작성하는 C++ 코드도 마찬가지겠지 😂

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/practice/BP_RPS)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   