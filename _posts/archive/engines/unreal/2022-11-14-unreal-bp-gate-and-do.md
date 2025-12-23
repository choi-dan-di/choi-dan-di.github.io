---
title: "[Unreal Engine] BP - Gate, MultiGate, Do Once, Do N"
excerpt: "조건에 따라 실행 흐름을 제어할 수 있는 노드(Gate, MultiGate, Do Once, Do N)에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, gate, multigate, do once, do n]

permalink: /unreal/bp-gate-and-do/

toc: true
toc_sticky: true

date: 2022-11-14 20:29:02+0900
last_modified_at: 2022-11-14 20:29:05+0900
---

## 👻 들어가기에 앞서
이번 시간에는 우리가 다루지 않았던 나머지 노드들에대해 알아볼 예정이다. ``` Gate ```, ``` MultiGate ```, ``` Do Once ```, ``` Do N ``` 총 네가지를 다룰 예정이며 이전에 알아봤던 노드들과 같이 조건에 따라 실행 흐름을 제어하는 분기문에 해당한다.

***

## 👻 Gate
**성문**과 비슷한 느낌이라 생각하면 된다. 문이 열려있으면 그대로 통과되고, 문이 닫혀있으면 통과되지 않는 기능을 나타낸다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/gate.PNG)   

> - **Enter** : 출력값을 나타내줌
- **Open** : 게이트를 열어줌
- **Close** : 게이트를 닫음
- **Toggle** : 게이트가 열려있으면 닫히고, 닫혀있으면 열림
- **Start Closed** : 시작 시 게이트 개방 여부
- **Exit** : 출력값

- 응용   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/gate2.PNG)   

위의 캡쳐처럼 블루프린트를 만들어두고 실행해보자.   

처음에는 **닫힌 상태로 시작**하니 아무리 출력을 하라는 의미의 1번 키를 눌러도 반응이 없지만, 2번 키를 눌러서 게이트를 연 후 다시 1번 키를 누르게되면 ``` Hello ```가 잘 출력된다. 마찬가지로 3번 키는 게이트 닫음, 4번 키는 게이트가 닫혀있으면 열어주고, 열려있으면 닫는 토글 역할을 하게된다.

***

## 👻 MultiGate
여러개의 성문이라고 생각하면 된다. 기본적으로 모든 성문은 열려있다가 하나씩 통과할 때마다 닫힌다. ``` Out ```은 **Add pin plus**로 여러개 추가할 수 있고 입력값을 주게되면 0번부터 순차적으로 지나가게 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/multigate.PNG)   

> - Empty Input : 출력값을 나타내줌
- **Reset** : Out 순서를 초기화시킴
- **Is Random** : Out 순서를 무작위로 변경
- **Loop** : 게이트 닫힘 없이 무한으로 출력함
- **Start Index** : 처음 출력할 Out의 인덱스 설정
- **Out** : 출력값

- 응용   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/multigate2.PNG)   

입력값을 이것저것 만져보며 테스트해보면 대략적인 멀티게이트의 흐름을 알 수 있다.

***

## 👻 Do Once
말 그대로 한 번만 실행시킨다는 의미이다. 게이트와 비슷하나 조금 더 축소된 기능을 가지고 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/do-once.PNG)   

> 입출력값은 ``` Gate```와 비슷하다.

- 응용   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/do-once2.PNG)   

6번 키를 누르면 한 번만 실행되고 더 이상 실행되지 않는다. 물론 리셋도 가능하다.

***

## 👻 Do N
Do Once와는 다르게 N번 실행시킨다는 의미이다. 원하는 개수만큼 출력시킨 후 출력을 막는다. ``` For Loop ```과 비슷하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/do-n.PNG)   

> - **N** : N번 실행하라는 의미
- **Counter** : 현재 몇 번째인지 나타내줌

- 응용   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-gate-and-do/do-n2.PNG)   

7번 키를 누르면 ``` 1, 2, 3, 4, 5```로 총 5번 출력되고 더 이상 출력되지 않는다.

***

## 👻 글을 마치며
이번 시간에는 처음 보는 노드들에 대해 알게되었다. 처음에 봤을 때는 무슨 기능을 하는지 가늠을 못했지만 의미를 알고나니 이해가 되었다. 그러면서 동시에 의문도 들었다. 이런 기능은 어디서 사용될까하고. 아직까지 게임이 어떻게 만들어져있고 어떤 식으로 돌아가는지를 알지못하니 활용법은 당장에 기억나지 않았다. 그래도 외워두면 언젠가 사용하게 되면서 그제서야 이해를 하게 되는 날이 오지 않을까 싶다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/BP_GateAndDo)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   