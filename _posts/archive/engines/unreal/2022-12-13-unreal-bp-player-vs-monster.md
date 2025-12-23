---
title: "[Unreal Engine] BP - 연습 문제 : Player vs Monster"
excerpt: "연습 문제를 통해 객체 지향 과정 익혀보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, oop, practice, player, monster]

permalink: /unreal/bp-player-vs-monster/
 
toc: true
toc_sticky: true

date: 2022-12-13 19:22:16+0900
last_modified_at: 2022-12-13 19:22:19+0900
---

## 👻 Player vs Monster
플레이어와 몬스터가 전투하는 기능을 구현해보자.

***

### 🌱 클래스 생성
Player와 Monster의 클래스를 각각 생성해주고 ``` Hp ```, ``` Damage ``` 변수를 만들어 각각의 체력과 공격력을 설정해주었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/player-class.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/monster-class.PNG)   

***

### 🌱 초기 세팅
다시 맵으로 돌아와 생성한 객체를 하나씩 세팅하고 레벨 블루프린트로 넘어와 마찬가지로 변수를 설정해주었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/main.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/level-bp.PNG)   

***

### 🌱 기능 구현
이제 **1번**을 누르면 **플레이어가 공격**을, **2번**을 누르면 **몬스터가 공격**을 하는 기능을 구현해 볼 것이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/bp2.PNG)   

위의 이미지처럼 간단하게 구현할 수 있다. 하지만 우리가 앞서 배웠던 객체 지향을 활용하면 각 클래스 내에서 해당 기능을 구현할 수도 있다.

- ``` Monster ``` **클래스**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/monster-ondamaged.PNG)   

- ``` Player ``` **클래스**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/player-ondamaged.PNG)   

- **블루프린트**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/bp3.PNG)   

- **결과**

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/oop/practice/bp-player-vs-monster/result.PNG)   

> 피격처럼 다음과 같이 두 클래스 간의 이벤트를 구성할 땐 이벤트를 **받는 입장**에서 정리하는 것이 편하다.

***

## 👻 글을 마치며
이번 시간에는 연습 문제를 통해 각 클래스 간의 이벤트를 구성하는 방법을 알아보았다. 말로는 쉬워보여도 기능을 구현하는 건 조금 더 많은 시간과 생각이 필요한 것 같다. 쉽게 구현할 수 있는 기능이라도 어떻게하면 조금 더 효율적으로 메모리 사용을 덜 하면서 만들지 생각하다보면 아무리 쉬운 기능이라도 어렵게 느껴지는 것 같다. 처음엔 우선적으로 기능을 구현하고 추후 다듬어가는 과정이 필요할 듯 하다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/oop/practice/BP_PlayerVSMonster)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   