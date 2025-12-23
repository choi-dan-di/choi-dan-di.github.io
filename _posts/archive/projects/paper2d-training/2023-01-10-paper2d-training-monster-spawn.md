---
title: "[Paper2D Training] #9. 몬스터 스폰하기"
excerpt: "게임이 시작하면 몬스터가 스폰되도록 변경하기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, spawn]

permalink: /paper2d-training/monster-spawn/

toc: true
toc_sticky: true

date: 2023-01-10 22:55:27+0900
last_modified_at: 2023-01-10 22:55:31+0900
---

## 👻 스폰하기
게임을 시작하면 플레이어만 생성된다. 몬스터를 스폰하기 위해 코드를 추가해보자.

***

### 🌱 Random Position
타일맵에서 유효한 타일의 좌표값을 구하고 해당 지점에 몬스터를 스폰할 것이다. 반복문을 통해 랜덤 값을 생성하고 해당 좌표의 타일이 유효하면 반복문을 빠져나온 후 스폰 지점을 결정하게 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/get-random-empty-grid-pos.PNG)   

말 그대로 비어있는 타일의 좌표값을 랜덤으로 구하는 함수이다. 이제 몬스터를 생성하는 함수를 만들어보자.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/spawn-creature.PNG)   

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/class-reference.PNG)   

반환 타입은 **오브젝트가 아닌 클래스** 타입으로 만들어 준다. 해당 클래스의 액터를 인게임에 출력해야하기 때문이다. 이제 테스트를 해보기 위해 타일맵이 생성될 때 실행될 ``` Event BeginPlay ```에 테스트 코드를 추가하고 확인해보자.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/spawn-test.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/spawn-monster.PNG)   

***

## 👻 유닛 충돌
현재 타일맵의 타일 단위로 충돌을 관리하고 있기 때문에 이전의 캡슐 콜리젼은 이제 더 이상 필요없어졌다. 플레이어의 앞 쪽에 위치한 타일에 몬스터가 있는지만 판단하면 되기 때문이다. 그래서 플레이어의 캡슐 컴포넌트 충돌 프리셋을 ``` NoCollision ```으로 변경해주고 충돌 방식을 수정해 줄 것이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/no-collision.PNG)   

위처럼 수정하게되면 처음 통맵으로 개발했을 때처럼 유닛이 겹치지 않고 지나치게 된다. 다시 충돌 기능을 추가해줘야하는데 기존의 캡슐 충돌과 관련된 모든 노드를 지우고 타일을 확인하여 동작을 수행할 수 있도록 변경해보자.

***

### 🌱 유닛 찾기
타일 좌표 값을 이용해 해당 위치에 유닛이 존재하는지 판단하는 함수를 하나 생성해주었다. 그 전에, 타일맵의 모든 유닛을 관리할 수 있도록 ``` BP_Creature ``` 타입의 배열을 만들고, 유닛을 생성할 때 모두 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/get-creature-at-grid-pos.PNG)   

각 유닛은 생성과 동시에 좌표값이 세팅된다. 입력받은 좌표값을 검색하여 유닛이 존재하면 Found = True와 유닛 자체를 반환하는 기능을 한다.

***

### 🌱 Can Go
이제 유닛이 이동할 곳이 유효한지 확인하는 함수 ``` Can Go ```의 조건문도 살짝 변경해줘야한다. 일단 유효한 타일인지 먼저 체크한 후, 해당 타일에 유닛이 존재하는지도 확인해줘야한다. 위에서 만든 ``` Get Creature At Grid Pos ``` 함수를 이용하여 쉽게 확인할 수 있다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/can-go.PNG)   

***

### 🌱 Process Attack
마지막으로 ``` Process Attack ``` 함수를 수정해줘야한다. 유닛 충돌 방식을 타일 단위로 변경하면서 Attack Range도 삭제시켰다. 유닛의 앞 타일을 확인하여 있으면 공격하는 방식으로 변경하기 위해서다. **Attack Range에 들어오는 액터를 찾아 데미지를 입히는 방식**에서 **해당 좌표 값의 유닛 여부를 판단하여 존재하면 데미지를 입히는 방식**으로 변경해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/process-attack.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-spawn/move-attack.gif)   

***

## 👻 글을 마치며
이번 시간에는 몬스터를 스폰하고 충돌 방식을 캐릭터에서 타일 방식으로 변경해보았다. 한 칸씩 이동하는 기능을 구현할 때까지만 해도 어려웠었는데 이해하고 진도를 계속 나가니 오히려 코드가 간결해지고 가독성이 좋아진 것 같다. 기능 구현도 훨씬 간결하고 이해도 잘 되는 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_