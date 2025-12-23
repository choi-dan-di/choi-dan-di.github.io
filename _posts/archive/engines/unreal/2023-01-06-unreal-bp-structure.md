---
title: "[Unreal Engine] BP - 구조체(Structure)"
excerpt: "블루프린트에서의 구조체에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, vector, rotator, structure]

permalink: /unreal/bp-structure/

toc: true
toc_sticky: true

date: 2023-01-06 15:53:59+0900
last_modified_at: 2023-01-06 15:54:03+0900
---

## 👻 구조체
**구조체(Structure)**는 **연관성이 있는 다양한 유형의 데이터 모음**이다. 객체의 이동, 회전 등을 담당하는 벡터, 로테이터, 트랜스폼이 구조체에 속한다.

***

### 🌱 구조체 생성 및 설정

블루프린트 클래스, 인터페이스와 같이 쉽게 만들 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/structure.PNG)   

구조체를 만든 후 더블클릭하여 상세 페이지로 들어가면 묶어서 관리할 데이터들을 추가할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/variable.PNG)   

***

### 🌱 구조체 사용
레벨 블루프린트로 돌아와 방금 만든 구조체를 사용해보자. 변수를 하나 추가해 해당 변수의 타입을 구조체로 설정하면 쉽게 만들 수 있고 우측 화면에서 기본 값의 세팅이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/variable2.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/stat.PNG)   

다른 변수와 똑같이 **get, set** 노드를 사용할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/get-set.PNG)   

다만, 여기서 차이점은 구조체 안엔 여러개의 데이터가 묶여있기 때문에 **핀을 분리시키거나 재결합**시킬 수 있다. 핀을 우클릭하면 분리, 재결합이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/split.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/split-pin.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/recombine.PNG)   

핀을 분리시키지 않고 단순 **get, set** 노드를 이용하여 개별의 데이터에 접근하고 싶다면 다음의 노드들을 이용한다.

- **Set members in Structure**   
구조체를 연결한 후, 우측의 **Default Category**를 체크하면 각각의 데이터에 접근해 값을 변경할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/set-members.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/default-category.PNG)   

- **Break Structure**   
구조체를 연결한 후, 각각의 데이터에 따로 접근할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/break.PNG)   

- **Make Structure**   
초기 구조체의 값을 세팅하여 하나의 구조체로 리턴시키는 노드이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/make-structure.PNG)   

이전 시간에 클래스를 공부하면서 사용했던 위치 세팅 노드인 **Make Transform**도 구조체에 속하며 x, y, z 좌표를 생성해 하나의 구조체로 리턴시키는 기능을 한다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-structure/make-transform.PNG)   

***

## 👻 글을 마치며
이번 시간에는 구조체에 대해 알아보았다. 구조체란 이름만 들어보고 기능은 정확히 알지 못했는데 이번 시간에 확실하게 알 수 있었다. 구조체를 잘 사용하면 데이터 관리가 용이해질 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/vector-rotator/BP_Structure)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   