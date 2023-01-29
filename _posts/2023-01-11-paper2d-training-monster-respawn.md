---
title: "[Paper2D Training] #11. 몬스터 리스폰하기"
excerpt: "몬스터 디스폰 이후 리스폰하기"

categories:
  - Paper2D Training
tags:
  - [Paper2D Training, Paper2D, 2d, ue, ue5, unreal engine, spawn, despawn, respawn]

permalink: /paper2d-training/monster-respawn/

toc: true
toc_sticky: true

date: 2023-01-11 02:55:32+0900
last_modified_at: 2023-01-11 02:55:35+0900
---

## 👻 몬스터 리스폰하기
인게임에서 스폰되는 몬스터를 카운터하여 무한으로 리스폰되는 기능을 만들어보자. ``` TileMap ``` 클래스에 ``` Actor Component ```를 추가하여 무한 리스폰 기능을 부품 조립하듯이 붙일 것이다. 컴포넌트를 만드는 방법은 이전과 같다. 컴포넌트를 상속받은 블루프린트 클래스를 만들어주면 된다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-respawn/create-component.PNG)   

컴포넌트에는 **액터 컴포넌트**와 **씬 컴포넌트**가 있다. 설명을 읽어보면 말 그대로 액터 컴포넌트는 **어느 액터에나 추가할 수 있는 재사용 가능한 컴포넌트**이다. 즉 액터에 다양한 기능을 조립하여 사용 가능하도록 해주는 것이다. 씬 컴포넌트는 **변형이 가능하고 다른 씬 컴포넌트에 붙여서 사용**할 수 있는 컴포넌트이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-respawn/add-component.PNG)   

두 가지 컴포넌트를 ``` Creature ```에 추가해보았다. 씬 컴포넌트는 위쪽에 추가되어 다른 컴포넌트 밑으로 들어갈 수도 있지만 액터 컴포넌트는 상속 관계를 만들지 못하고 독립적으로 움직인다. 액터 컴포넌트를 하나 만들고 몬스터를 무한 리스폰하는 기능을 부여해보자.

***

### 🌱 무한 리스폰
우선 ``` TileMap ```이 액터 컴포넌트를 들고 있기 때문에 해당 클래스 내에서 액터 컴포넌트를 언제든지 사용할 수 있고, 반대로 액터 컴포넌트에서는 ``` Get Owner ``` 노드를 이용해 상위 클래스인 ``` TileMap ```에 접근할 수 있다. 이 곳에 추가한 이유는 해당 클래스가 유닛 스폰, 디스폰과 관련된 함수를 가지고 있기 때문이다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-respawn/get-owner.PNG)   

인게임에서 몬스터의 제한은 2마리로 설정하고 스폰, 디스폰 될 때마다 카운터를 조정해 주도록 하였다. ``` Despawn Creature ``` 함수에 유닛 카운트를 1 감소 시키는 기능을 가진 함수를 추가해주었다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-respawn/despawn-creature.PNG)   

그리고 커스텀 이벤트를 하나 만들어 무한 루프를 돌려주었다. 단, 총 유닛 개수보다 현재 유닛 개수가 적을 때에만 리스폰 되도록 제한을 걸어두었기 때문에 최대 유닛 개수에 도달하면 더 이상 리스폰되지 않고 일정 시간 이후 다시 자기 자신의 이벤트를 호출하는 방식으로 구현하였다.

![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-respawn/update-random-spawn.PNG)   

- **결과**   
![Alt Text](/assets/images/posts_img/projects/paper2d-training/monster-respawn/update-random-spawn.gif)   

***

## 👻 글을 마치며
이번 시간에는 컴포넌트를 이용하여 몬스터가 리스폰되는 기능을 구현해보았다. 생각보다 간단하고 컴포넌트를 잘 이용하니 상당히 편리한 것 같았다. 아직 정확히 어떤 기능까지 가능할지 모르지만 웬만하면 원하는대로 개발할 수 있을 것 같다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/ji8q)_