---
title: "[Paper2D Training] #10. 몬스터 AI 재구현, 디스폰하기"
excerpt: "몬스터에 AI를 다시 적용시키고 디스폰하기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, spawn, despawn, ai]

permalink: /paper2d-training/monster-ai-and-despawn/

toc: true
toc_sticky: true

date: 2023-01-11 00:45:13+0900
last_modified_at: 2023-01-11 00:45:17+0900
---

## 👻 몬스터 AI
이전 시간에 ``` Chase Direction ```, 단위 벡터 등을 이용해 몬스터가 플레이어를 추적할 수 있도록 만들었었다. 플레이어 입장에서의 충돌을 타일 단위로 바꾼 것처럼 몬스터도 타일 단위로 확인하고 판단하여 플레이어를 추적할 수 있도록 코드를 변경할 것이다.

***

### 🌱 컨트롤러 연결
인게임에 드래그 앤 드롭으로 세팅한 몬스터는 미리 설정을 해두었기 때문에 ``` AIController ```가 적용이 됐었다. 하지만 코드를 통해 스폰을 한 몬스터들은 컨트롤러가 연결이 되어있지 않아 에러를 발생시킨다. 이 에러를 수정하기 위해 ``` Event BeginPlay ``` 대신 ``` Event On Possess ```를 사용하여 스폰과 동시에 컨트롤러와 연결할 수 있도록 수정해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/event-on-possess.PNG)   

> 💡 **Possess 뜻**   
붙잡다, 소유하다, ...에 붙다, 악령 따위가 ...에 붙다(빙의하다)

***

### 🌱 타일 개수 세기
몬스터로부터 플레이어까지의 거리를 개수로 반환해 줄 함수를 하나 생성했다. 플레이어의 좌표값에서 몬스터의 좌표값을 빼면 각 좌표의 **델타값**을 구할 수 있다. 이 수들을 전부 합하면 총 타일 개수가 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/get-tile-count-to-target.PNG)   

타일 개수가 1이면 바로 **옆 타일에 위치**한다는 의미이다.

***

### 🌱 플레이어 바라보기
그 다음으로는 몬스터가 플레이어를 바라봐야하기 때문에 ``` Look At Target ```이라는 함수를 만들어 플레이어 쪽을 바라보도록 해주는 함수를 만들어서 적용시켰다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/look-at-target.PNG)   

X 축과 Y 축의 변화량을 구해 변수에 저장하고, 조금 더 멀리 떨어져있는 축을 우선으로 몬스터의 방향을 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/look-at-target2.PNG)   

맵 좌표와 벡터 좌표를 헷갈리지 않도록 주의해야한다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/monster-ai-error.gif)   

잘 따라오는 것 같긴하나 제대로 기능이 동작하지 않는 것을 알 수 있다. 이러한 이유는 이전 시간에 플레이어의 입장에서 수정한 코드가 몬스터에는 적용되지 않기 때문이다. ``` Stop Move ``` 변수는 오로지 플레이어만 사용하는 변수기 때문에 코드가 제대로 동작하지 않는 것이다.

***

### 🌱 함수 재정의
이러한 버그를 해결하기 위해 ``` Update Destination ``` 함수를 재정의 해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/update-destination.PNG)   

먼저 플레이어가 있는 방향으로 수정해준 뒤 부모의 ``` Update Destination ``` 함수를 실행시켜주면 정상적으로 목적지가 세팅된 것을 알 수 있고 ``` Update AI ```를 마지막으로 다음과 같이 수정해줌으로써 몬스터의 AI 기능을 재구현 할 수 있게 되었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/update-ai.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/monster-ai.gif)   

***

## 👻 몬스터 디스폰하기
몬스터의 체력이 0이 되면 디스폰하는 기능을 구현해보자.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/despawn-creature.PNG)   

우선 함수를 새로 생성해 유닛이 유효한지 체크하고, 유효하다면 ``` Creatures ```에서 삭제 후 ``` Destroy Actor ``` 노드를 사용하여 인게임에서도 삭제시키는 기능을 구현해준다.

***

### 🌱 On Dead
디스폰하는 함수를 만들었고 이 함수를 언제 사용할지 확인하여 연결만 시켜주면 간단하게 완성된다. 우선, 체력이 변하는 ``` On Damaged ``` 함수 내에서 체력 비율이 0이면 ``` On Dead ```라는 함수를 실행시키는 기능을 구현할 것이다. ``` On Dead ``` 함수 내에서 ``` Despawn Creature ``` 함수를 호출한다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/add-on-dead.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/on-dead.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/despawn-monster.gif)   

***

## 👻 UI 추가하기
몬스터를 죽이면 킬 수를 증가시키고, 그 수를 UI 상에 나타내보자. **위젯 블루프린트**를 사용하여 ``` Game UI ``` 블루프린트를 생성한 후 캔버스와 텍스트를 이용해 간단히 출력하는 기능까지 구현해보았다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/game-ui.PNG)   

- **결과 화면**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/killcount-ui.PNG)   

텍스트에는 포맷 기능을 추가한 새로운 함수를 만들어 바인딩해주었다. 지금은 간단하게 타일맵을 관리하는 클래스에서 위젯을 만들고 추가하는 기능을 구현해보았다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/set-game-ui.PNG)   

이제 ``` On Dead ``` 함수에서 ``` KillCount ```를 수정하면 킬 수가 증가하게 될 것이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/on-dead2.PNG)   

현재 코드는 비효율 적이고 간단하게 맛만 보기 위해 사용한 코드이다.

> **Get Actor Of Class** 노드는 효율성이 별로 좋지 않다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-ai-and-despawn/kill-count.gif)   

***

## 👻 글을 마치며
지난 시간에 만들어 보았던 몬스터 AI 기능을 타일 단위를 판단하는 방식으로 재구현해보고 체력이 0이 되면 디스폰되는 기능을 구현해 보았다. 유저가 직접 조종하는 유닛이 아니다보니 생각해야 할 게 두 배로 많았던 것 같다. 다른 방법으로도 구현할 수 있는지 조금 더 생각 해봐야겠다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_